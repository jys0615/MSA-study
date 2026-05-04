# Troubleshooting

개발하면서 마주한 문제와 해결 과정을 기록합니다.

---

## v0.1 — Gradle 멀티모듈 세팅

---

### [TS-001] `implementation()` 메서드를 찾을 수 없음

**증상**
```
Could not find method implementation() for arguments [...]
on object of type org.gradle.api.internal.artifacts.dsl.dependencies.DefaultDependencyHandler.
```

**원인**

`implementation`은 `java` 플러그인이 제공하는 의존성 구성(configuration)입니다.
서브모듈의 `build.gradle`에 `plugins {}` 블록 없이 `dependencies {}` 블록만 작성하면, `java` 플러그인이 적용되지 않은 상태에서 `implementation`을 찾으므로 에러가 발생합니다.

초기 시도한 방식 — 루트 `build.gradle`의 `subprojects {}` 블록에서 `apply plugin: 'java'`를 선언하는 방식은 Gradle 설정 평가 순서에 따라 서브모듈 `build.gradle`보다 늦게 적용될 수 있어 신뢰할 수 없습니다.

**해결**

각 서브모듈의 `build.gradle`에 `plugins {}` 블록을 직접 선언합니다.

```groovy
// ❌ 잘못된 방식 — plugins 블록 없이 dependencies만 선언
dependencies {
    implementation 'org.springframework.cloud:spring-cloud-starter-netflix-eureka-server'
}

// ✅ 올바른 방식 — plugins 블록 먼저 선언
plugins {
    id 'java'
    id 'org.springframework.boot'
    id 'io.spring.dependency-management'
}

dependencies {
    implementation 'org.springframework.cloud:spring-cloud-starter-netflix-eureka-server'
}
```

**교훈**

Gradle 멀티모듈에서 루트의 `subprojects {}` 블록은 공통 설정(group, version, repositories)에만 사용하고, 플러그인은 각 서브모듈에서 명시적으로 선언하는 것이 명확하고 안전합니다.

---

### [TS-002] 파일명에 공백이 포함된 `build.gradle `

**증상**

루트 `build.gradle`이 존재함에도 Gradle이 해당 파일을 인식하지 못하고 설정이 적용되지 않음.

**원인**

IntelliJ에서 파일을 생성할 때 파일명 끝에 공백이 포함되어 `build.gradle ` (trailing space)로 저장됐습니다. `ls` 명령으로는 구분이 어렵고 `ls -la`로 확인 가능했습니다.

```bash
# ❌ 공백 포함된 파일명 (Gradle이 인식 못함)
-rw-r--r--  build.gradle 

# ✅ 정상 파일명
-rw-r--r--  build.gradle
```

**해결**

```bash
mv 'build.gradle ' 'build.gradle'
```

**교훈**

빌드 설정 파일이 존재하는데 Gradle이 인식하지 못할 때는 `ls -la`로 파일명의 숨겨진 문자(공백, 특수문자)를 확인합니다.

---

### [TS-003] IntelliJ Gradle JVM 불일치 경고

**증상**
```
해당 작업에 현재 선택된 Gradle JVM을 사용할 수 없습니다.
/opt/homebrew/opt/java/libexec/openjdk.jdk/Contents/Home의 설치본이 대신 사용됩니다.
```

**원인**

IntelliJ의 Gradle 설정에서 지정한 JVM 경로와 실제 설치된 JDK 경로가 다릅니다.
빌드 자체는 Homebrew로 설치된 JDK로 대체 실행되어 성공하지만, 경고가 계속 출력됩니다.

**해결**

`IntelliJ → Settings → Build, Execution, Deployment → Build Tools → Gradle → Gradle JVM`을 `/opt/homebrew/opt/java/libexec/openjdk.jdk/Contents/Home`으로 명시적으로 지정합니다.

**현재 상태** : 빌드는 정상 동작 중. JVM 경로 설정은 선택 사항.

---

*새로운 트러블슈팅은 `[TS-NNN]` 형식으로 추가합니다.*
