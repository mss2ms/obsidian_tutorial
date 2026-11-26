
## 1. JavaScript Execution

- **RunJS**는 노트의 코드 블록이나 `.js`/`.mjs` 파일을 실행합니다. 명령 팔레트, 리본 아이콘, 자동 시작을 지원합니다.
- **Execute Code**는 코드 블록에 "run" 버튼을 붙입니다. JS 외에 Python, Rust, Shell 등도 되며, JS는 Node.js가 필요합니다.
- **Interactivity**는 텍스트 패턴이나 단축키로 로컬 셸 명령과 스크립트를 실행합니다.

## 2. CustomJS

**동작 방식**
- 설정에서 JS 파일 경로를 지정하거나 폴더를 지정하면 `.js` 파일을 읽어 로드합니다.[2]
- 파일마다 클래스 하나를 정의하고, 인스턴스가 `customJS.클래스명`으로 노출됩니다.[2]
- 모듈은 CommonJS만 지원합니다.[3]
- 전역 `customJS`에는 `app`과 `obsidian` 객체가 들어 있습니다.[2]
- `.js` 파일을 수정하면 자동으로 다시 로드되고, Dataview 블록도 갱신됩니다.[2]

**호출 예시**
Dataview에서는 `await customJS.MacroCategoriesOfPage.listInBox(dv, "경로")`처럼 호출합니다. 이 패턴은 JS를 마크다운 밖의 스크립트 파일로 빼서 관리하는 방식입니다.[4]

**부가 기능**
- `invoke()` 메서드가 있는 클래스는 "Invoke Script" 명령으로 실행할 수 있고, 핫키로 등록할 수도 있습니다.[2][5]
- 시작 스크립트(startup)를 지정하면 Obsidian 실행 시 자동으로 돌아갑니다.[2]
- `deconstructor()`를 정의하면 언로드나 재로드 때 정리 작업을 할 수 있습니다.[2]
- 모바일에서도 동작합니다.[6]

**대안**
TypeScript나 ESM이 필요하면 **CodeScript Toolkit**이 있습니다. 클래스 하나당 파일 하나라는 제약이 없고, CustomJS·Dataview·QuickAdd·Templater를 확장합니다. **Modules** 플러그인은 볼트와 인터넷에서 JS/TS 모듈을 불러옵니다.[7][8]

## 3. QuickAdd

- **Macro**에서 여러 단계를 순서대로 실행하고 데이터를 넘깁니다.
- **User Script**는 `.js` 파일이나 노트의 ```js 블록(첫 블록만 실행)에 `module.exports = async (params) => {...}` 형태로 씁니다. `params`로 `app`, `quickAddApi`, `variables`를 받습니다.
- `obsidian vault=dev quickadd choice="Daily log"`처럼 CLI에서 실행할 수 있습니다.

## 4. Plugin Programming

이 부분은 검색 결과가 아니라 일반 지식입니다. TypeScript와 `obsidian-sample-plugin`으로 시작하고, `Plugin` 클래스의 `onload()`에서 명령과 설정 탭을 등록합니다. 빌드 결과물인 `main.js`, `manifest.json`, `styles.css`는 `.obsidian/plugins/<id>/`에 둡니다.

## 선택 기준

| 목적 | 추천 |
|---|---|
| 간단한 스니펫 실험 | RunJS, Execute Code |
| Dataview/Templater에서 공용 함수 재사용 | CustomJS |
| 단축키 기반 자동화 워크플로 | QuickAdd Macro + User Script |
| TS/ESM 등 모던 모듈 구조 | CodeScript Toolkit |
| 배포나 UI가 필요한 기능 | 플러그인 개발 |

CustomJS와 QuickAdd는 비슷해 보이지만 쓰임이 다릅니다. CustomJS는 "코드 라이브러리"이고, QuickAdd는 "트리거와 워크플로"입니다. 둘을 같이 쓰는 경우도 많습니다. 예를 들어 QuickAdd 스크립트에서 `customJS` 함수를 불러 쓸 수 있습니다.

Sources
[1] CustomJS - Obsidian Plugin https://community.obsidian.md/plugins/customjs
[2] obsidian-custom-js/main.ts at master https://github.com/SamLewis0602/obsidian-custom-js/blob/master/main.ts
[3] Macros | QuickAdd https://quickadd.obsidian.guide/docs/Choices/MacroChoice/
[4] Using custom scripts in Obsidian | - Code In Focus https://codeinfocus.com/blog/2024-12/using-custom-scripts-in-obsidian/
[5] Guide to develop and execute modular JavaScript in Obsidian https://forum.obsidian.md/t/guide-to-develop-and-execute-modular-javascript-in-obsidian/88339
[6] User Scripts https://quickadd.obsidian.guide/docs/UserScripts/
[7] Obsidian plugins with #scripting - 6 plugins https://www.obsidianstats.com/tags/scripting
[8] quickadd.obsidian.guide › docs › Advancedquickadd.obsidian.guide https://quickadd.obsidian.guide/docs/Advanced/CLI.md
[9] customjs - Obsidian Hub https://publish.obsidian.md/hub/02+-+Community+Expansions/02.05+All+Community+Expansions/Plugins/customjs
[10] CodeScript Toolkit - Obsidian Plugin https://community.obsidian.md/plugins/fix-require-modules
[11] Execute Code - Obsidian Plugin https://community.obsidian.md/plugins/execute-code
[12] Modules - Obsidian Plugin https://community.obsidian.md/plugins/modules
[13] CustomJS (DeepWiki) (websites/deepwiki_saml-dev_obsidian- ... https://context7.com/websites/deepwiki_saml-dev_obsidian-custom-js
[14] RunJS - Obsidian Plugin https://community.obsidian.md/plugins/runjs
[15] A tutorial on customjs and fetching public API : r/ObsidianMD https://www.reddit.com/r/ObsidianMD/comments/15vtwbn/a_tutorial_on_customjs_and_fetching_public_api/
[16] Interactivity: Calculations and Scripts - Obsidian Plugin https://community.obsidian.md/plugins/interactivity
