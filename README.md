```
Paradise-Seeker/
├── assets/
│   ├── audio/
│   │   ├── bgm/
│   │   ├── sfx/
│   │   └── ending/
│   ├── fonts/
│   ├── maps/
│   ├── textures/
│   │   ├── character/
│   │   ├── monster/
│   │   ├── npc/
│   │   ├── object/
│   │   ├── ui/
│   │   └── cutscene/
│   │       ├── intro/
│   │       ├── route_normal/
│   │       ├── route_true/
│   │       └── route_observer/
│   └── story/
│       ├── dialogue/
│       │   ├── common/
│       │   ├── normal/
│       │   ├── true/
│       │   └── observer/
│       ├── ending/
│       │   ├── ending_normal.json
│       │   ├── ending_true.json
│       │   └── ending_observer.json
│       └── events/
│           ├── npc_events.json
│           ├── boss_events.json
│           └── route_conditions.json
│
├── core/
│   └── src/main/java/com/paradise_seeker/game/
│       ├── collision/
│       ├── entity/
│       │   ├── monster/
│       │   ├── npc/
│       │   ├── player/
│       │   └── Character.java
│       │
│       ├── event/                         // mới
│       │   ├── GameEvent.java
│       │   ├── EventBus.java
│       │   ├── RouteEvent.java
│       │   ├── BossDefeatedEvent.java
│       │   ├── NPCInteractedEvent.java
│       │   └── PortalTriggeredEvent.java
│       │
│       ├── main/
│       │   └── Main.java
│       │
│       ├── map/
│       │   ├── GameMap.java
│       │   ├── GameMap1.java
│       │   ├── GameMap2.java
│       │   ├── GameMap3.java
│       │   ├── GameMap4.java
│       │   ├── GameMap5.java
│       │   ├── GameMapManager.java
│       │   ├── MonsterFactory.java
│       │   └── SolidObject.java
│       │
│       ├── meta/                          // mới
│       │   ├── MetaDetector.java
│       │   ├── TamperRule.java
│       │   ├── SystemSignature.java
│       │   └── ObserverTrigger.java
│       │
│       ├── object/
│       │   ├── item/
│       │   ├── Book.java
│       │   ├── Chest.java
│       │   ├── GameObject.java
│       │   └── Portal.java
│       │
│       ├── rendering/
│       │   ├── animations/
│       │   ├── effects/
│       │   ├── renderer/
│       │   ├── MonsterHPBarManager.java
│       │   └── Renderable.java
│       │
│       ├── save/                          // mới
│       │   ├── SaveData.java
│       │   ├── SaveManager.java
│       │   ├── PreferencesStore.java
│       │   └── RouteSnapshot.java
│       │
│       ├── screen/
│       │   ├── cutscene/
│       │   │   ├── CutScene.java
│       │   │   ├── IntroCutScene.java
│       │   │   ├── EndMap1.java
│       │   │   ├── EndMap2.java
│       │   │   ├── EndMap3.java
│       │   │   ├── EndMap4.java
│       │   │   ├── EndingCutscene.java           // thay cho EndGame cứng
│       │   │   ├── NormalEndingCutscene.java
│       │   │   ├── TrueEndingCutscene.java
│       │   │   └── ObserverEndingCutscene.java
│       │   ├── ControlScreen.java
│       │   ├── DeadScreen.java
│       │   ├── GameScreen.java
│       │   ├── InventoryScreen.java
│       │   ├── MainMenuScreen.java
│       │   ├── PauseScreen.java
│       │   ├── SettingScreen.java
│       │   └── WinScreen.java
│       │
│       ├── story/                         // mới, quan trọng nhất
│       │   ├── RouteType.java             // NORMAL / TRUE / OBSERVER
│       │   ├── StoryFlag.java
│       │   ├── StoryState.java
│       │   ├── StoryStateManager.java
│       │   ├── RouteResolver.java
│       │   ├── EndingResolver.java
│       │   ├── dialogue/
│       │   │   ├── DialogueLine.java
│       │   │   ├── DialogueSequence.java
│       │   │   ├── DialogueRepository.java
│       │   │   └── DialogueService.java
│       │   └── condition/
│       │       ├── StoryCondition.java
│       │       ├── FlagCondition.java
│       │       ├── BossCondition.java
│       │       └── MetaCondition.java
│       │
│       ├── test/
│       │   ├── GameTest.java
│       │   ├── StoryStateManagerTest.java
│       │   ├── RouteResolverTest.java
│       │   └── MetaDetectorTest.java
│       │
│       └── ui/
│           ├── DialogueBox.java
│           ├── ChoiceBox.java              // mới, nếu muốn lựa chọn route
│           ├── RouteHintWidget.java        // mới
│           └── EndingSummaryPanel.java     // mới
│
├── lwjgl3/
├── gradle/
├── build.gradle
├── settings.gradle
├── README.md
└── docs/                                   // nên thêm
    ├── story-outline.md
    ├── route-design.md
    ├── ending-design.md
    └── task-breakdown.md
```
