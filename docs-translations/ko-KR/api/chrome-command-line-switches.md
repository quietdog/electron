﻿# 크롬 명령줄 스위치 지원

크롬 명령줄(Command-Line) 스위치는 크롬 브라우저에서 제공되는 추가 옵션이며
Electron에서도 지원합니다. [app][app]의 [ready][ready]이벤트가 작동하기 전에
[app.commandLine.appendSwitch][append-switch] API를 사용하면 어플리케이션 내부에서
스위치를 추가할 수 있습니다:

```javascript
const app = require('electron').app;
app.commandLine.appendSwitch('remote-debugging-port', '8315');
app.commandLine.appendSwitch('host-rules', 'MAP * 127.0.0.1');

app.on('ready', function() {
  // Your code here
});
```

## --client-certificate=`path`

`path`를 클라이언트 인증서로 설정합니다.

## --ignore-connections-limit=`domains`

`domains` 리스트(`,`로 구분)의 연결 제한을 무시합니다.

## --disable-http-cache

HTTP 요청 캐시를 비활성화 합니다.

## --remote-debugging-port=`port`

지정한 `port`에 HTTP 기반의 리모트 디버거를 활성화 시킵니다. (개발자 도구)

## --js-flags=`flags`

JS 엔진에 지정한 플래그를 전달합니다. `flags`를 메인 프로세스에서 활성화하고자 한다면,
Electron이 시작되기 전에 스위치를 전달해야 합니다.

```bash
$ electron --js-flags="--harmony_proxies --harmony_collections" your-app
```

## --proxy-server=`address:port`

시스템 설정의 프록시 서버를 무시하고 지정한 서버로 연결합니다. HTTP와 HTTPS 요청에만
적용됩니다.

시스템 프록시 서버 설정을 무시하고 지정한 서버로 연결합니다. 이 스위치는 HTTP와 HTTPS
그리고 WebSocket 요청에만 적용됩니다. 그리고 모든 프록시 서버가 HTTPS가 WebSocket
요청을 지원하지 않고 있을 수 있으므로 사용시 주의해야 합니다.

## --proxy-bypass-list=`hosts`

Electron이 세미콜론으로 구분된 호스트 리스트에서 지정한 프록시 서버를 건너뛰도록
지시합니다.

예시:

```javascript
app.commandLine.appendSwitch('proxy-bypass-list', '<local>;*.google.com;*foo.com;1.2.3.4:5678')`
```

위 예시는 로컬 주소(`localhost`, `127.0.0.1`, 등)와 `google.com`의 서브도메인,
`foo.com`을 접미사로 가지는 호스트, `1.2.3.4:5678` 호스트를 제외한 모든 연결에서
프록시 서버를 사용합니다.

## --proxy-pac-url=`url`

지정한 `url`의 PAC 스크립트를 사용합니다.

## --no-proxy-server

프록시 서버를 사용하지 않습니다. 다른 프록시 서버 플래그 및 설정을 무시하고 언제나 직접
연결을 사용합니다.

## --host-rules=`rules`

Hostname 맵핑 규칙을 설정합니다. (`,`로 구분)

예시:

* `MAP * 127.0.0.1` 강제적으로 모든 호스트네임을 127.0.0.1로 맵핑합니다
* `MAP *.google.com proxy` 강제적으로 모든 google.com의 서브도메인을 "proxy"로
  연결합니다
* `MAP test.com [::1]:77` 강제적으로 "test.com"을 IPv6 루프백으로 연결합니다.
  소켓 주소의 포트 또한 77로 고정됩니다.
* `MAP * baz, EXCLUDE www.google.com` "www.google.com"을 제외한 모든 것들을
  "baz"로 맵핑합니다.

이 맵핑은 네트워크 요청시의 endpoint를 지정합니다. (TCP 연결과 직접 연결의 호스트
resolver, http 프록시 연결의 `CONNECT`, `SOCKS` 프록시 연결의 endpoint 호스트)

## --host-resolver-rules=`rules`

`--host-rules` 플래그와 비슷하지만 이 플래그는 host resolver에만 적용됩니다.

## --ignore-certificate-errors

인증서 에러를 무시합니다.

## --ppapi-flash-path=`path`

Pepper 플래시 플러그인의 위치를 설정합니다.

## --ppapi-flash-version=`version`

Pepper 플래시 플러그인의 버전을 설정합니다.

## --log-net-log=`path`

Net log 이벤트를 활성화하고 `path`에 로그를 기록합니다.

## --ssl-version-fallback-min=`version`

TLS fallback에서 사용할 SSL/TLS 최소 버전을 지정합니다. ("tls1", "tls1.1",
"tls1.2")

## --cipher-suite-blacklist=`cipher_suites`

SSL 암호화를 비활성화할 대상 목록을 지정합니다. (`,`로 구분)

## --enable-logging

Chromium의 로그를 콘솔에 출력합니다.

이 스위치는 어플리케이션이 로드되기 전에 분석 되므로 `app.commandLine.appendSwitch`
메서드에선 사용할 수 없습니다. 하지만 `ELECTRON_ENABLE_LOGGING` 환경 변수를 설정하면
본 스위치를 지정한 것과 같은 효과를 낼 수 있습니다.

## --v=`log_level`

기본 V-logging 최대 활성화 레벨을 지정합니다. 기본값은 0입니다. 기본적으로 양수를
레벨로 사용합니다.

이 스위치는 `--enable-logging` 스위치를 같이 지정해야 작동합니다.

## --vmodule=`pattern`

`--v` 옵션에 전달된 값을 덮어쓰고 모듈당 최대 V-logging 레벨을 지정합니다.
예를 들어 `my_module=2,foo*=3`는 `my_module.*`, `foo*.*`와 같은 파일 이름 패턴을
가진 모든 소스 코드들의 로깅 레벨을 각각 2와 3으로 설정합니다.

또한 슬래시(`/`) 또는 백슬래시(`\`)를 포함하는 패턴은 지정한 경로에 대해 패턴을 테스트
합니다. 예를 들어 `*/foo/bar/*=2` 표현식은 `foo/bar` 디렉터리 안의 모든 소스 코드의
로깅 레벨을 2로 지정합니다.

이 스위치는 `--enable-logging` 스위치를 같이 지정해야 작동합니다.

[app]: app.md
[append-switch]: app.md#appcommandlineappendswitchswitch-value
[ready]: app.md#event-ready
