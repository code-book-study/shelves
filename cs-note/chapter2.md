# chapter2 네트워크
# 2장 – 네트워크 면접 질문 뱅크

---

## 생각해볼 질문들
# 네트워크 면접 핵심 주제 & 예상 질문

| 주제 | 면접 예상 질문 |
|------|---------------|
| **HTTP/2·HTTP/3 <br/>& QUIC** | • HTTP/2 멀티플렉싱이 헤드-오브-라인 블로킹(HOL)을 완화하는 과정을 설명하세요.<br>• HTTP/3가 UDP 기반 0-RTT와 TLS 통합을 달성한 흐름은?<br>• Server Push 사용 시 성능이 오히려 저하되는 경우는 언제인가요? |
| **HTTPS & TLS <br/>핸드셰이크 최적화** | • TLS 1.3이 왕복 횟수를 줄인 메커니즘은?<br>• HSTS·HPKP·Certificate Pinning을 실무에서 적용한 경험이 있나요?<br>• Mixed-Content 차단 및 CSP 설정을 어떻게 대응했나요? |
| **브라우저 캐싱 & CDN** | • `Cache-Control`, `ETag`, Service Worker 업데이트 시나리오를 비교해 보세요.<br>• 전역 코드-스플리팅 번들 vs CDN Edge 캐싱의 트레이드오프는?<br>• Stale-While-Revalidate 전략을 SPA에서 어떻게 구현했나요? |
| **TCP 3-Way Handshake <br/>& 초기 지연** | • TCP Slow-Start가 대역폭을 점진적으로 늘리는 과정을 설명하세요.<br>• HTTP/3 0-RTT 재사용이 모바일 재접속에 유리한 이유는?<br>• TTFB·RTT가 Core Web Vitals(LCP 등)에 어떻게 영향을 주나요? |
| **DevTools Network 패널 <br/>& 측정 도구** | • Waterfall 차트를 읽고 RTT와 Server-Timing 헤더를 해석하는 방법은?<br>• Lighthouse·WebPageTest·`web-vitals` JS로 성능을 측정하는 방법은?<br>• DevTools에서 HOLB·Blocking Time을 발견하고 개선한 경험이 있나요? |
| **DNS 과정 & DoH/DoT** | • 브라우저 DNS 캐시 → OS 캐시 → CDN 레코드 흐름을 설명하세요.<br>• DoH(HTTPS)·DoT가 기업 방화벽/콘텐츠 필터링에 미치는 영향은?<br>• Geo DNS·Anycast가 TTFB를 단축하는 원리를 설명해 보세요. |

<br/>

---

<!-- TEAM_LINKS_START -->

## 팀원별 정리 링크

- [이명욱-chapter2](이명욱/cs-note/chapter2.md)
- [이동현-chapter2](이동현/cs-note/chapter2.md)
<!-- TEAM_LINKS_END -->
