<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# 이 세션에서 이어서할게

좋아. **이 세션에서 그대로 이어가자.** 앞서 보낸 코드도 다시 올릴 필요 없어.

## 현재까지 확인한 내용

- `DAT_71028966a8`은 초기화 경로에서 `1`, 다른 설정 경로에서 `param_7 > 1`이면 `2`, 아니면 `1`로 설정돼.
- 추적한 설정값은 `60 / 값`과 `값 / 60` 계산에 사용돼.
- `FUN_71006c2580`에는 실제 경과 시간·고정 간격·외부 설정값 중 하나를 선택하는 분기가 있어.
- 그 하위의 `FUN_710010ece0 → FUN_710010f440`에서는 시간값을 타이머에 사용하는 것을 확인했어. 다만 이 하위 처리는 오디오 관리로 추정되며, 게임 전체의 이동·물리 시간이라고 확정하지 않았어.
- 아직 패치 위치는 확정하지 않았고, 누락 구간도 당장 다시 보낼 필요 없어.


## 이어서 볼 지점

다음 추적 대상은 `FUN_71006c2580`에서 선택한 시간값을 별도 객체에 저장하는 부분이야.

```c
if (param_1[6] != 0) {
    *(float *)(*(long *)(param_1[6] + 0x28) + 0x20) = fVar70;
}
```

현재 객체의 `+0x30`에 어떤 참조 객체가 들어오는지 확인하는 단계에서 멈춰 있어. 생성자나 초기화 함수를 아직 찾지 않았다면, 찾았다고 가정하지 않고 그 위치를 찾는 작업부터 이어가면 돼.

---

# FUN_71006c2580 어떻게 찾아가지?

Ghidra에서 **`G`** 키로 바로 이동하면 돼.

1. 가운데 Listing 창을 한 번 클릭해.
2. 키보드에서 `G`를 눌러.
3. 입력창에 아래 주소를 붙여 넣고 Enter를 눌러.

```text
71006c2580
```


`Go To`는 주소나 함수 이름으로 이동하는 기능이야. 이동하면 오른쪽 Decompiler에도 해당 함수가 표시될 거야.[^2_1][^2_2]

이 함수 전체는 이미 받았으니 다시 복사할 필요 없어. 우선 화면에 `FUN_71006c2580`이 뜨는 것까지만 하면 돼.

<span style="display:none">[^2_10][^2_3][^2_4][^2_5][^2_6][^2_7][^2_8][^2_9]</span>

<div align="center">⁂</div>

[^2_1]: https://www.ghidradocs.com/11.4_PUBLIC/help/Base/help/topics/Navigation/Navigation.htm

[^2_2]: https://ghidradocs.com/12.1.2_PUBLIC/help/Decompiler/help/topics/DecompilePlugin/DecompilerIntro.html

[^2_3]: https://reverseengineering.stackexchange.com/questions/30378/how-to-reference-an-address-set-by-2-instructions-in-ghidra

[^2_4]: https://ghidra.re/ghidra_docs/GhidraClass/Beginner/Introduction_to_Ghidra_Student_Guide.html

[^2_5]: https://www.ghidradocs.com/9.0.4_PUBLIC/help/Base/help/topics/Navigation/Navigation.htm

[^2_6]: https://github.com/zernanvash/cheatsheet/blob/main/reverse-engineering-vault/tools/Ghidra Cheat Sheet.md

[^2_7]: https://github.com/Ilias1988/Hacking-Cheatsheets/blob/main/Ghidra/README.md

[^2_8]: https://github.com/NationalSecurityAgency/ghidra/blob/c4273522/Ghidra/Debug/Debugger/src/main/help/help/topics/DebuggerListingPlugin/DebuggerListingPlugin.html

[^2_9]: https://www.ghidradocs.com/10.2.3_PUBLIC/help/Decompiler/help/topics/DecompilePlugin/DecompilerIntro.html

[^2_10]: https://github.com/MBenDelphi/NDXDeveloper.reverse-engineering-gcc-gpp-training/blob/main/08-ghidra/03-codebrowser-navigation.md


---

# ```

                             LAB_71006c25f8                                  XREF[1]:     71006c25dc(j)  
      71006c25f8 34 69 40 a9     ldp        x20,x26,[x9]
      71006c25fc e9 13 00 f9     str        x9,[sp, #local_e0]
      71006c2600 9f 02 00 f1     cmp        x20,#0x0
      71006c2604 44 1b 40 fa     ccmp       x26,#0x0,#0x4,ne
      71006c2608 a1 00 00 54     b.ne       LAB_71006c261c
      71006c260c 9a 01 00 b5     cbnz       x26,LAB_71006c263c
      71006c2610 28 00 80 52     mov        w8,#0x1
      71006c2614 a8 c3 1b b8     stur       w8,[x29, #local_a4]
      71006c2618 32 00 00 14     b          LAB_71006c26e0
                             LAB_71006c261c                                  XREF[1]:     71006c2608(j)  
      71006c261c 68 02 40 f9     ldr        x8,[x19]
      71006c2620 e0 03 13 aa     mov        param_1,x19
      71006c2624 08 15 40 f9     ldr        x8,[x8, #0x28]
      71006c2628 00 01 3f d6     blr        x8
      71006c262c e2 03 00 aa     mov        param_3,param_1
      71006c2630 e0 03 1a aa     mov        param_1,x26
      71006c2634 e1 03 14 aa     mov        param_2,x20
      71006c2638 e6 51 ea 97     bl         FUN_7100156dd0                                   undefined FUN_7100156dd0(long pa
                             LAB_71006c263c                                  XREF[1]:     71006c260c(j)  
      71006c263c 48 b7 40 39     ldrb       w8,[x26, #0x2d]
      71006c2640 88 00 38 36     tbz        w8,#0x7,LAB_71006c2650
      71006c2644 28 fb 41 b9     ldr        w8,[x25, #0x1f8]
      71006c2648 08 01 1f 32     orr        w8,w8,#0x2
      71006c264c 28 fb 01 b9     str        w8,[x25, #0x1f8]
                             LAB_71006c2650                                  XREF[1]:     71006c2640(j)  
      71006c2650 68 b2 40 f9     ldr        x8,[x19, #0x160]
      71006c2654 48 01 00 b4     cbz        x8,LAB_71006c267c
      71006c2658 00 15 40 f9     ldr        param_1,[x8, #0x28]
      71006c265c 48 fb 44 b9     ldr        w8,[x26, #0x4f8]
      71006c2660 1f 05 00 71     cmp        w8,#0x1
      71006c2664 48 1f 40 b9     ldr        w8,[x26, #0x1c]
      71006c2668 08 25 09 53     ubfx       w8,w8,#0x9,#0x1
      71006c266c e1 13 88 1a     csel       param_2+0x4,wzr,w8,ne
      71006c2670 08 00 40 f9     ldr        x8,[param_1]
      71006c2674 08 ad 42 f9     ldr        x8,[x8, #0x558]
      71006c2678 00 01 3f d6     blr        x8
                             LAB_71006c267c                                  XREF[1]:     71006c2654(j)  
      71006c267c 22 84 8b 52     mov        param_3+0x4,#0x5c21
      71006c2680 e0 03 19 aa     mov        param_1,x25
      71006c2684 e1 03 15 aa     mov        param_2,x21
      71006c2688 e2 2f bc 72     movk       param_3+0x4,#0xe17f, LSL #16
      71006c268c e4 03 1f 2a     mov        w4,wzr
      71006c2690 cc 4b e5 97     bl         FUN_71000155c0                                   long FUN_71000155c0(long param_1
      71006c2694 40 02 00 b4     cbz        param_1,LAB_71006c26dc
      71006c2698 08 10 40 f9     ldr        x8,[param_1, #0x20]
      71006c269c f4 03 00 aa     mov        x20,param_1
      71006c26a0 68 01 00 b4     cbz        x8,LAB_71006c26cc
      71006c26a4 88 06 40 f9     ldr        x8,[x20, #0x8]
      71006c26a8 28 01 00 b4     cbz        x8,LAB_71006c26cc
      71006c26ac 00 15 40 f9     ldr        param_1,[x8, #0x28]
      71006c26b0 e0 00 00 b4     cbz        param_1,LAB_71006c26cc
      71006c26b4 48 ab 45 b9     ldr        w8,[x26, #0x5a8]
      71006c26b8 1f 05 00 71     cmp        w8,#0x1
      71006c26bc 08 00 40 f9     ldr        x8,[param_1]
      71006c26c0 e1 17 9f 1a     cset       param_2+0x4,eq
      71006c26c4 08 d5 42 f9     ldr        x8,[x8, #0x5a8]
      71006c26c8 00 01 3f d6     blr        x8
                             LAB_71006c26cc                                  XREF[3]:     71006c26a0(j), 71006c26a8(j), 
                                                                                          71006c26b0(j)  
      71006c26cc e0 03 14 aa     mov        param_1,x20
      71006c26d0 e1 03 15 aa     mov        param_2,x21
      71006c26d4 e2 03 19 aa     mov        param_3,x25
      71006c26d8 8a bb e5 97     bl         FUN_7100031500                                   undefined FUN_7100031500(uint * 
                             LAB_71006c26dc                                  XREF[1]:     71006c2694(j)  
      71006c26dc bf c3 1b b8     stur       wzr,[x29, #local_a4]
                             LAB_71006c26e0                                  XREF[1]:     71006c2618(j)  
      71006c26e0 68 02 40 f9     ldr        x8,[x19]
      71006c26e4 e0 03 13 aa     mov        param_1,x19
      71006c26e8 08 15 40 f9     ldr        x8,[x8, #0x28]
      71006c26ec 00 01 3f d6     blr        x8
      71006c26f0 68 02 40 f9     ldr        x8,[x19]
      71006c26f4 e0 17 00 f9     str        param_1,[sp, #local_d8]
      71006c26f8 e0 03 13 aa     mov        param_1,x19
      71006c26fc 08 21 40 f9     ldr        x8,[x8, #0x40]
      71006c2700 00 01 3f d6     blr        x8
      71006c2704 e8 c2 40 b9     ldr        w8,[x23, #0xc0]
      71006c2708 f3 0b 00 f9     str        x19,[sp, #local_f0]
      71006c270c c0 00 00 b4     cbz        param_1,LAB_71006c2724
      71006c2710 68 00 00 35     cbnz       w8,LAB_71006c271c
      71006c2714 68 33 40 b9     ldr        w8,[x27, #0x30]
      71006c2718 e8 c2 00 b9     str        w8,[x23, #0xc0]
                             LAB_71006c271c                                  XREF[1]:     71006c2710(j)  
      71006c271c 08 00 40 b9     ldr        w8,[param_1]
      71006c2720 02 00 00 14     b          LAB_71006c2728
                             LAB_71006c2724                                  XREF[1]:     71006c270c(j)  
      71006c2724 48 00 00 34     cbz        w8,LAB_71006c272c
                             LAB_71006c2728                                  XREF[1]:     71006c2720(j)  
      71006c2728 68 33 00 b9     str        w8,[x27, #0x30]
                             LAB_71006c272c                                  XREF[1]:     71006c2724(j)  
      71006c272c e8 12 43 39     ldrb       w8,[x23, #0xc4]
      71006c2730 e8 0f 00 b9     str        w8,[sp, #local_f4]
      71006c2734 e8 b2 40 b9     ldr        w8,[x23, #0xb0]
      71006c2738 28 02 00 34     cbz        w8,LAB_71006c277c
      71006c273c f3 03 1f aa     mov        x19,xzr
      71006c2740 14 f1 7d d3     lsl        x20,x8,#0x3
      71006c2744 04 00 00 14     b          LAB_71006c2754
                             LAB_71006c2748                                  XREF[2]:     71006c275c(j), 71006c2778(j)  
      71006c2748 73 22 00 91     add        x19,x19,#0x8
      71006c274c 9f 02 13 eb     cmp        x20,x19
      71006c2750 60 01 00 54     b.eq       LAB_71006c277c
                             LAB_71006c2754                                  XREF[1]:     71006c2744(j)  
      71006c2754 e8 1e 40 f9     ldr        x8,[x23, #0x38]
      71006c2758 08 69 73 f8     ldr        x8,[x8, x19, LSL #0x0]
      71006c275c 68 ff ff b4     cbz        x8,LAB_71006c2748
      71006c2760 00 15 40 f9     ldr        param_1,[x8, #0x28]
      71006c2764 e1 03 15 aa     mov        param_2,x21
      71006c2768 e2 03 19 aa     mov        param_3,x25
      71006c276c 08 00 40 f9     ldr        x8,[param_1]
      71006c2770 08 25 40 f9     ldr        x8,[x8, #0x48]
      71006c2774 00 01 3f d6     blr        x8
      71006c2778 f4 ff ff 17     b          LAB_71006c2748
                             LAB_71006c277c                                  XREF[2]:     71006c2738(j), 71006c2750(j)  
      71006c277c e8 ba 40 b9     ldr        w8,[x23, #0xb8]
      71006c2780 a8 02 00 34     cbz        w8,LAB_71006c27d4
      71006c2784 f3 03 1f aa     mov        x19,xzr
      71006c2788 14 f1 7d d3     lsl        x20,x8,#0x3
      71006c278c 08 00 00 14     b          LAB_71006c27ac
      71006c2790 1f              ??         1Fh
      71006c2791 20              ??         20h     
      71006c2792 03              ??         03h
      71006c2793 d5              ??         D5h
      71006c2794 1f              ??         1Fh
      71006c2795 20              ??         20h     
      71006c2796 03              ??         03h
      71006c2797 d5              ??         D5h
      71006c2798 1f              ??         1Fh
      71006c2799 20              ??         20h     
      71006c279a 03              ??         03h
      71006c279b d5              ??         D5h
      71006c279c 1f              ??         1Fh
      71006c279d 20              ??         20h     
      71006c279e 03              ??         03h
      71006c279f d5              ??         D5h
                             LAB_71006c27a0                                  XREF[2]:     71006c27b4(j), 71006c27d0(j)  
      71006c27a0 73 22 00 91     add        x19,x19,#0x8
      71006c27a4 9f 02 13 eb     cmp        x20,x19
      71006c27a8 60 01 00 54     b.eq       LAB_71006c27d4
                             LAB_71006c27ac                                  XREF[1]:     71006c278c(j)  
      71006c27ac e8 26 40 f9     ldr        x8,[x23, #0x48]
      71006c27b0 08 69 73 f8     ldr        x8,[x8, x19, LSL #0x0]
      71006c27b4 68 ff ff b4     cbz        x8,LAB_71006c27a0
      71006c27b8 00 15 40 f9     ldr        param_1,[x8, #0x28]
      71006c27bc e1 03 15 aa     mov        param_2,x21
      71006c27c0 e2 03 19 aa     mov        param_3,x25
      71006c27c4 08 00 40 f9     ldr        x8,[param_1]
      71006c27c8 08 25 40 f9     ldr        x8,[x8, #0x48]
      71006c27cc 00 01 3f d6     blr        x8
      71006c27d0 f4 ff ff 17     b          LAB_71006c27a0
                             LAB_71006c27d4                                  XREF[2]:     71006c2780(j), 71006c27a8(j)  
      71006c27d4 e8 be 40 b9     ldr        w8,[x23, #0xbc]
      71006c27d8 28 02 00 34     cbz        w8,LAB_71006c281c
      71006c27dc f3 03 1f aa     mov        x19,xzr
      71006c27e0 14 f1 7d d3     lsl        x20,x8,#0x3
      71006c27e4 04 00 00 14     b          LAB_71006c27f4
                             LAB_71006c27e8                                  XREF[2]:     71006c27fc(j), 71006c2818(j)  
      71006c27e8 73 22 00 91     add        x19,x19,#0x8
      71006c27ec 9f 02 13 eb     cmp        x20,x19
      71006c27f0 60 01 00 54     b.eq       LAB_71006c281c
                             LAB_71006c27f4                                  XREF[1]:     71006c27e4(j)  
      71006c27f4 e8 2a 40 f9     ldr        x8,[x23, #0x50]
      71006c27f8 08 69 73 f8     ldr        x8,[x8, x19, LSL #0x0]
      71006c27fc 68 ff ff b4     cbz        x8,LAB_71006c27e8
      71006c2800 00 15 40 f9     ldr        param_1,[x8, #0x28]
      71006c2804 e1 03 15 aa     mov        param_2,x21
      71006c2808 e2 03 19 aa     mov        param_3,x25
      71006c280c 08 00 40 f9     ldr        x8,[param_1]
      71006c2810 08 25 40 f9     ldr        x8,[x8, #0x48]
      71006c2814 00 01 3f d6     blr        x8
      71006c2818 f4 ff ff 17     b          LAB_71006c27e8
                             LAB_71006c281c                                  XREF[2]:     71006c27d8(j), 71006c27f0(j)  
      71006c281c 68 33 40 b9     ldr        w8,[x27, #0x30]
      71006c2820 fa 2b 00 f9     str        x26,[sp, #local_b0]
      71006c2824 b9 83 1c f8     stur       x25,[x29, #local_98]
      71006c2828 1f 05 00 71     cmp        w8,#0x1
      71006c282c 13 85 9f 1a     csinc      w19,w8,wzr,hi
      71006c2830 b8 4d 60 94     bl         <EXTERNAL>::nn::os::GetSystemTickFrequency       undefined GetSystemTickFrequency
      71006c2834 61 03 43 2d     ldp        s1,s0,[x27, #0x18]
      71006c2838 fa 03 00 aa     mov        x26,param_1
      71006c283c 7c 23 40 a9     ldp        x28,x8,[x27]
      71006c2840 61 83 03 2d     stp        s1,s0,[x27, #0x1c]
      71006c2844 61 83 44 2d     ldp        s1,s0,[x27, #0x24]
      71006c2848 61 03 05 2d     stp        s1,s0,[x27, #0x28]
      71006c284c 7c a3 00 a9     stp        x28,x8,[x27, #0x8]
      71006c2850 c4 4d 60 94     bl         <EXTERNAL>::nn::os::GetSystemTick                undefined GetSystemTick(void)
      71006c2854 08 00 1c eb     subs       x8,param_1,x28
      71006c2858 b5 03 1d f8     stur       x21,[x29, #local_90]
      71006c285c b7 03 1c f8     stur       x23,[x29, #local_a0]
      71006c2860 e8 1b 00 f9     str        x8,[sp, #local_d0]
      71006c2864 f3 03 00 f9     str        x19,[sp]=>local_100
      71006c2868 f6 0f 00 f9     str        x22,[sp, #local_e8]
      71006c286c fb 1f 00 f9     str        x27,[sp, #local_c8]
      71006c2870 c9 15 00 54     b.ls       LAB_71006c2b28
      71006c2874 54 0b d3 9a     udiv       x20,x26,x19
      71006c2878 e9 f9 9e d2     mov        x9,#0xf7cf
      71006c287c 48 ff 43 d3     lsr        x8,x26,#0x3
      71006c2880 69 6a bc f2     movk       x9,#0xe353, LSL #16
      71006c2884 36 00 80 52     mov        w22,#0x1
      71006c2888 1b 10 80 52     mov        w27,#0x80
      71006c288c a9 74 d3 f2     movk       x9,#0x9ba5, LSL #32
      71006c2890 89 18 e4 f2     movk       x9,#0x20c4, LSL #48
      71006c2894 b4 83 1d f8     stur       x20,[x29, #local_88]
      71006c2898 08 7d c9 9b     umulh      x8,x8,x9
      71006c289c 08 fd 44 d3     lsr        x8,x8,#0x4
      71006c28a0 f3 03 08 aa     mov        x19,x8
      71006c28a4 e8 6b 04 a9     stp        x8,x26,[sp, #local_c0]
      71006c28a8 0a 00 00 14     b          LAB_71006c28d0
                             LAB_71006c28ac                                  XREF[1]:     71006c28e4(j)  
      71006c28ac a1 22 40 b9     ldr        param_2+0x4,[x21, #0x20]
      71006c28b0 a2 43 00 d1     sub        param_3,x29,#0x10
      71006c28b4 e0 03 19 aa     mov        param_1,x25
      71006c28b8 e3 03 1f 2a     mov        param_4+0x4,wzr
      71006c28bc b6 03 1f 38     sturb      w22,[x29, #local_70]
      71006c28c0 b0 f6 e6 97     bl         FUN_7100080380                                   undefined FUN_7100080380(long pa
                             LAB_71006c28c4                                  XREF[1]:     71006c2b24(j)  
      71006c28c4 a7 4d 60 94     bl         <EXTERNAL>::nn::os::GetSystemTick                undefined GetSystemTick(void)
      71006c28c8 1f 00 1c eb     cmp        param_1,x28
      71006c28cc e9 12 00 54     b.ls       LAB_71006c2b28
                             LAB_71006c28d0                                  XREF[1]:     71006c28a8(j)  
      71006c28d0 08 00 1c cb     sub        x8,param_1,x28
      71006c28d4 88 02 08 eb     subs       x8,x20,x8
      71006c28d8 89 12 00 54     b.ls       LAB_71006c2b28
      71006c28dc b9 06 40 f9     ldr        x25,[x21, #0x8]
      71006c28e0 1f 01 13 eb     cmp        x8,x19
      71006c28e4 43 fe ff 54     b.cc       LAB_71006c28ac
      71006c28e8 a8 22 40 b9     ldr        w8,[x21, #0x20]
      71006c28ec a2 43 00 d1     sub        param_3,x29,#0x10
      71006c28f0 e0 03 19 aa     mov        param_1,x25
      71006c28f4 37 13 42 f9     ldr        x23,[x25, #0x420]
      71006c28f8 24 00 80 52     mov        w4,#0x1
      71006c28fc b6 03 1f 38     sturb      w22,[x29, #local_70]
      71006c2900 14 05 00 51     sub        w20,w8,#0x1
      71006c2904 e3 03 17 aa     mov        param_4,x23
      71006c2908 e1 03 14 2a     mov        param_2+0x4,w20
      71006c290c 3d f4 e6 97     bl         FUN_710007fa00                                   int FUN_710007fa00(long param_1,
      71006c2910 1f 20 03 d5     nop
      71006c2914 1f 20 03 d5     nop
      71006c2918 1f 20 03 d5     nop
      71006c291c 1f 20 03 d5     nop
                             LAB_71006c2920                                  XREF[1]:     71006c29dc(j)  
      71006c2920 e8 02 40 f9     ldr        x8,[x23]
      71006c2924 e8 05 00 b4     cbz        x8,LAB_71006c29e0
      71006c2928 e8 0a 08 8b     add        x8,x23,x8, LSL #0x2
      71006c292c eb 03 1f aa     mov        x11,xzr
      71006c2930 ec 03 1f aa     mov        x12,xzr
      71006c2934 e9 03 08 aa     mov        x9,x8
      71006c2938 2a 8d 42 f8     ldr        x10,[x9, #0x28]!
      71006c293c ea 02 00 f9     str        x10,[x23]
      71006c2940 29 81 ff f8     swpal      xzr,x9,[x9]
      71006c2944 09 21 40 b9     ldr        w9,[x8, #0x20]
      71006c2948 3f 01 02 71     cmp        w9,#0x80
      71006c294c 29 b1 9b 1a     csel       w9,w9,w27,lt
      71006c2950 2a 05 00 51     sub        w10,w9,#0x1
      71006c2954 29 0b 42 f9     ldr        x9,[x25, #0x410]
      71006c2958 2d cd 2a 8b     add        x13,x9,w10, SXTW  #0x3
      71006c295c 29 1f 42 f9     ldr        x9,[x25, #0x438]
      71006c2960 bf 3b 03 d5     dmb        ISH
      71006c2964 ab fd eb c8     casal      x11,x11,[x13]
      71006c2968 0e 31 40 b9     ldr        w14,[x8, #0x30]
      71006c296c cf 05 00 11     add        w15,w14,#0x1
      71006c2970 ce 7d 60 d3     lsl        x14,x14,#0x20
      71006c2974 0f 31 00 b9     str        w15,[x8, #0x30]
      71006c2978 0f 01 0d cb     sub        x15,x8,x13
      71006c297c ee 85 42 b3     bfxil      x14,x15,#0x2,#0x20
                             LAB_71006c2980                                  XREF[1]:     71006c2998(j)  
      71006c2980 10 a1 00 91     add        x16,x8,#0x28
      71006c2984 ef 03 0b aa     mov        x15,x11
      71006c2988 0b fe ec c8     casal      x12,x11,[x16]
      71006c298c ec 03 0f aa     mov        x12,x15
      71006c2990 ae fd eb c8     casal      x11,x14,[x13]
      71006c2994 ff 01 0b eb     cmp        x15,x11
      71006c2998 41 ff ff 54     b.ne       LAB_71006c2980
      71006c299c 4b 7d 00 11     add        w11,w10,#0x1f
      71006c29a0 5f 01 00 71     cmp        w10,#0x0
      71006c29a4 e8 03 1f 2a     mov        w8,wzr
      71006c29a8 6b b1 8a 1a     csel       w11,w11,w10,lt
      71006c29ac ca 22 ca 1a     lsl        w10,w22,w10
      71006c29b0 6c 7d 05 13     asr        w12,w11,#0x5
      71006c29b4 2c c9 2c 8b     add        x12,x9,w12, SXTW  #0x2
      71006c29b8 6b 7d 45 93     sbfx       x11,x11,#0x5,#0x1b
      71006c29bc 88 fd e8 88     casal      w8,w8,[x12]
      71006c29c0 ec 03 08 2a     mov        w12,w8
                             LAB_71006c29c4                                  XREF[1]:     71006c29d8(j)  
      71006c29c4 2e 09 0b 8b     add        x14,x9,x11, LSL #0x2
      71006c29c8 0d 01 0a 2a     orr        w13,w8,w10
      71006c29cc cd fd ec 88     casal      w12,w13,[x14]
      71006c29d0 1f 01 0c 6b     cmp        w8,w12
      71006c29d4 e8 03 0c 2a     mov        w8,w12
      71006c29d8 61 ff ff 54     b.ne       LAB_71006c29c4
      71006c29dc d1 ff ff 17     b          LAB_71006c2920
                             LAB_71006c29e0                                  XREF[1]:     71006c2924(j)  
      71006c29e0 00 0a 00 37     tbnz       param_1+0x4,#0x0,LAB_71006c2b20
      71006c29e4 28 07 40 f9     ldr        x8,[x25, #0x8]
      71006c29e8 2a b7 8e 52     mov        w10,#0x75b9
      71006c29ec 2a ef a0 72     movk       w10,#0x779, LSL #16
      71006c29f0 08 71 40 f9     ldr        x8,[x8, #0xe0]
      71006c29f4 17 09 40 f9     ldr        x23,[x8, #0x10]
      71006c29f8 1a 29 40 b9     ldr        w26,[x8, #0x28]
      71006c29fc 29 03 17 cb     sub        x9,x25,x23
      71006c2a00 29 fd 43 d3     lsr        x9,x9,#0x3
      71006c2a04 33 7d 0a 1b     mul        w19,w9,w10
      71006c2a08 68 06 00 11     add        w8,w19,#0x1
      71006c2a0c 1f 01 1a 6b     cmp        w8,w26
      71006c2a10 f5 a7 93 1a     csinc      w21,wzr,w19,ge
      71006c2a14 bf 02 13 6b     cmp        w21,w19
      71006c2a18 80 01 00 54     b.eq       LAB_71006c2a48
      71006c2a1c 1f 20 03 d5     nop
                             LAB_71006c2a20                                  XREF[1]:     71006c2a44(j)  
      71006c2a20 08 89 80 52     mov        w8,#0x448
      71006c2a24 e1 03 14 2a     mov        param_2+0x4,w20
      71006c2a28 a0 5e 28 9b     smaddl     param_1,w21,w8,x23
      71006c2a2c 6d f5 e6 97     bl         FUN_710007ffe0                                   long FUN_710007ffe0(long param_1
      71006c2a30 80 01 00 b5     cbnz       param_1,LAB_71006c2a60
      71006c2a34 a8 06 00 11     add        w8,w21,#0x1
      71006c2a38 1f 01 1a 6b     cmp        w8,w26
      71006c2a3c f5 a7 95 1a     csinc      w21,wzr,w21,ge
      71006c2a40 bf 02 13 6b     cmp        w21,w19
      71006c2a44 e1 fe ff 54     b.ne       LAB_71006c2a20
                             LAB_71006c2a48                                  XREF[1]:     71006c2a18(j)  
      71006c2a48 00 a8 81 52     mov        param_1+0x4,#0xd40
      71006c2a4c 60 00 a0 72     movk       param_1+0x4,#0x3, LSL #16
      71006c2a50 cc 4c 60 94     bl         <EXTERNAL>::nn::os::SleepThread                  undefined SleepThread(TimeSpan p
      71006c2a54 b5 03 5d f8     ldur       x21,[x29, #local_90]
      71006c2a58 f3 6b 44 a9     ldp        x19,x26,[sp, #local_c0]
      71006c2a5c 31 00 00 14     b          LAB_71006c2b20
                             LAB_71006c2a60                                  XREF[1]:     71006c2a30(j)  
      71006c2a60 08 20 40 b9     ldr        w8,[param_1, #0x20]
      71006c2a64 ea 03 1f aa     mov        x10,xzr
      71006c2a68 eb 03 1f aa     mov        x11,xzr
      71006c2a6c 1f 01 02 71     cmp        w8,#0x80
      71006c2a70 08 b1 9b 1a     csel       w8,w8,w27,lt
      71006c2a74 09 05 00 51     sub        w9,w8,#0x1
      71006c2a78 28 0b 42 f9     ldr        x8,[x25, #0x410]
      71006c2a7c 0c cd 29 8b     add        x12,x8,w9, SXTW  #0x3
      71006c2a80 28 1f 42 f9     ldr        x8,[x25, #0x438]
      71006c2a84 bf 3b 03 d5     dmb        ISH
      71006c2a88 8a fd ea c8     casal      x10,x10,[x12]
      71006c2a8c 0d 30 40 b9     ldr        w13,[param_1, #0x30]
      71006c2a90 ae 05 00 11     add        w14,w13,#0x1
      71006c2a94 ad 7d 60 d3     lsl        x13,x13,#0x20
      71006c2a98 0e 30 00 b9     str        w14,[param_1, #0x30]
      71006c2a9c 0e 00 0c cb     sub        x14,param_1,x12
      71006c2aa0 cd 85 42 b3     bfxil      x13,x14,#0x2,#0x20
                             LAB_71006c2aa4                                  XREF[1]:     71006c2abc(j)  
      71006c2aa4 0f a0 00 91     add        x15,param_1,#0x28
      71006c2aa8 ee 03 0a aa     mov        x14,x10
      71006c2aac ea fd eb c8     casal      x11,x10,[x15]
      71006c2ab0 eb 03 0e aa     mov        x11,x14
      71006c2ab4 8d fd ea c8     casal      x10,x13,[x12]
      71006c2ab8 df 01 0a eb     cmp        x14,x10
      71006c2abc 41 ff ff 54     b.ne       LAB_71006c2aa4
      71006c2ac0 2b 7d 00 11     add        w11,w9,#0x1f
      71006c2ac4 3f 01 00 71     cmp        w9,#0x0
      71006c2ac8 ea 03 1f 2a     mov        w10,wzr
      71006c2acc 6b b1 89 1a     csel       w11,w11,w9,lt
      71006c2ad0 c9 22 c9 1a     lsl        w9,w22,w9
      71006c2ad4 6c 7d 05 13     asr        w12,w11,#0x5
      71006c2ad8 0c c9 2c 8b     add        x12,x8,w12, SXTW  #0x2
      71006c2adc 6b 7d 45 93     sbfx       x11,x11,#0x5,#0x1b
      71006c2ae0 8a fd ea 88     casal      w10,w10,[x12]
      71006c2ae4 b5 03 5d f8     ldur       x21,[x29, #local_90]
      71006c2ae8 f3 6b 44 a9     ldp        x19,x26,[sp, #local_c0]
      71006c2aec ec 03 0a 2a     mov        w12,w10
      71006c2af0 1f 20 03 d5     nop
      71006c2af4 1f 20 03 d5     nop
      71006c2af8 1f 20 03 d5     nop
      71006c2afc 1f 20 03 d5     nop
                             LAB_71006c2b00                                  XREF[1]:     71006c2b14(j)  
      71006c2b00 0e 09 0b 8b     add        x14,x8,x11, LSL #0x2
      71006c2b04 4d 01 09 2a     orr        w13,w10,w9
      71006c2b08 cd fd ec 88     casal      w12,w13,[x14]
      71006c2b0c 5f 01 0c 6b     cmp        w10,w12
      71006c2b10 ea 03 0c 2a     mov        w10,w12
      71006c2b14 61 ff ff 54     b.ne       LAB_71006c2b00
      71006c2b18 1f 20 03 d5     nop
      71006c2b1c 1f 20 03 d5     nop
                             LAB_71006c2b20                                  XREF[2]:     71006c29e0(j), 71006c2a5c(j)  
      71006c2b20 b4 83 5d f8     ldur       x20,[x29, #local_88]
      71006c2b24 68 ff ff 17     b          LAB_71006c28c4
                             LAB_71006c2b28                                  XREF[3]:     71006c2870(j), 71006c28cc(j), 
                                                                                          71006c28d8(j)  
      71006c2b28 f6 2b 43 a9     ldp        x22,x10,[sp, #local_d0]
      71006c2b2c 40 01 00 f9     str        param_1,[x10]
      71006c2b30 c1 02 63 9e     ucvtf      d1,x22
      71006c2b34 a9 03 5c f8     ldur       x9,[x29, #local_a0]
      71006c2b38 40 03 63 9e     ucvtf      d0,x26
      71006c2b3c e8 17 40 f9     ldr        x8,[sp, #local_d8]
      71006c2b40 21 18 60 1e     fdiv       d1,d1,d0
      71006c2b44 21 40 62 1e     fcvt       s1,d1
      71006c2b48 41 25 00 bd     str        s1,[x10, #0x24]
      71006c2b4c 68 00 00 b4     cbz        x8,LAB_71006c2b58
      71006c2b50 00 3d 40 bd     ldr        s0,[x8, #0x3c]
      71006c2b54 0c 00 00 14     b          LAB_71006c2b84
                             LAB_71006c2b58                                  XREF[1]:     71006c2b4c(j)  
      71006c2b58 e8 0f 40 b9     ldr        w8,[sp, #local_f4]
      71006c2b5c c8 00 00 37     tbnz       w8,#0x0,LAB_71006c2b74
      71006c2b60 08 00 1c cb     sub        x8,param_1,x28
      71006c2b64 01 01 63 9e     ucvtf      d1,x8
      71006c2b68 20 18 60 1e     fdiv       d0,d1,d0
      71006c2b6c 00 40 62 1e     fcvt       s0,d0
      71006c2b70 05 00 00 14     b          LAB_71006c2b84
                             LAB_71006c2b74                                  XREF[1]:     71006c2b5c(j)  
      71006c2b74 e8 03 40 f9     ldr        x8,[sp]=>local_100
      71006c2b78 01 10 2e 1e     fmov       s1,0x3f800000
      71006c2b7c 00 01 23 1e     ucvtf      s0,w8
      71006c2b80 20 18 20 1e     fdiv       s0,s1,s0
                             LAB_71006c2b84                                  XREF[2]:     71006c2b54(j), 71006c2b70(j)  
      71006c2b84 40 19 00 bd     str        s0,[x10, #0x18]
      71006c2b88 eb 13 40 f9     ldr        x11,[sp, #local_e0]
      71006c2b8c 28 19 40 f9     ldr        x8,[x9, #0x30]
      71006c2b90 68 00 00 b4     cbz        x8,LAB_71006c2b9c
      71006c2b94 08 15 40 f9     ldr        x8,[x8, #0x28]
      71006c2b98 00 21 00 bd     str        s0,[x8, #0x20]
                             LAB_71006c2b9c                                  XREF[1]:     71006c2b90(j)  
      71006c2b9c a8 c3 5b b8     ldur       w8,[x29, #local_a4]
      71006c2ba0 68 00 00 34     cbz        w8,LAB_71006c2bac
      71006c2ba4 60 85 52 bd     ldr        s0,[x11, #0x1284]
      71006c2ba8 05 00 00 14     b          LAB_71006c2bbc
                             LAB_71006c2bac                                  XREF[1]:     71006c2ba0(j)  
      71006c2bac e8 2b 40 f9     ldr        x8,[sp, #local_b0]
      71006c2bb0 60 85 52 bd     ldr        s0,[x11, #0x1284]
      71006c2bb4 08 91 40 39     ldrb       w8,[x8, #0x24]
      71006c2bb8 88 00 18 37     tbnz       w8,#0x3,LAB_71006c2bc8
                             LAB_71006c2bbc                                  XREF[1]:     71006c2ba8(j)  
      71006c2bbc 41 19 40 bd     ldr        s1,[x10, #0x18]
      71006c2bc0 00 28 21 1e     fadd       s0,s0,s1
      71006c2bc4 03 00 00 14     b          LAB_71006c2bd0
                             LAB_71006c2bc8                                  XREF[1]:     71006c2bb8(j)  
      71006c2bc8 01 e4 00 2f     movi       d1,#0x0
      71006c2bcc 5f 19 00 b9     str        wzr,[x10, #0x18]
                             LAB_71006c2bd0                                  XREF[1]:     71006c2bc4(j)  
      71006c2bd0 28 ac a8 52     mov        w8,#0x45610000
      71006c2bd4 02 01 27 1e     fmov       s2,w8
      71006c2bd8 00 20 22 1e     fcmp       s0,s2
      71006c2bdc 0d 01 00 54     b.le       LAB_71006c2bfc
      71006c2be0 28 ac b8 52     mov        w8,#0xc5610000
      71006c2be4 29 ac a8 52     mov        w9,#0x45610000
                             LAB_71006c2be8                                  XREF[1]:     71006c2bf8(j)  
      71006c2be8 02 01 27 1e     fmov       s2,w8
      71006c2bec 00 28 22 1e     fadd       s0,s0,s2
      71006c2bf0 22 01 27 1e     fmov       s2,w9
      71006c2bf4 00 20 22 1e     fcmp       s0,s2
      71006c2bf8 8c ff ff 54     b.gt       LAB_71006c2be8
                             LAB_71006c2bfc                                  XREF[1]:     71006c2bdc(j)  
      71006c2bfc 62 85 52 bd     ldr        s2,[x11, #0x1284]
      71006c2c00 08 f0 a7 52     mov        w8,#0x3f800000
      71006c2c04 60 85 12 bd     str        s0,[x11, #0x1284]
      71006c2c08 b3 83 5c f8     ldur       x19,[x29, #local_98]
      71006c2c0c 61 8d 12 bd     str        s1,[x11, #0x128c]
      71006c2c10 68 91 12 b9     str        w8,[x11, #0x1290]
      71006c2c14 e1 03 1f aa     mov        param_2,xzr
      71006c2c18 e2 03 13 aa     mov        param_3,x19
      71006c2c1c 62 89 12 bd     str        s2,[x11, #0x1288]
      71006c2c20 40 19 40 bd     ldr        s0,[x10, #0x18]
      71006c2c24 60 7e 40 f9     ldr        param_1,[x19, #0xf8]
      71006c2c28 2e 30 e9 97     bl         FUN_710010ece0                                   undefined FUN_710010ece0(float p
      71006c2c2c 68 ba 43 79     ldrh       w8,[x19, #0x1dc]
      71006c2c30 1f 0d 00 71     cmp        w8,#0x3
      71006c2c34 69 a2 40 f9     ldr        x9,[x19, #0x140]
      71006c2c38 29 61 06 91     add        x9,x9,#0x198
      71006c2c3c 33 81 9f 9a     csel       x19,x9,xzr,hi
      71006c2c40 5c 0a 00 b4     cbz        x28,LAB_71006c2d88
      71006c2c44 1f 11 00 71     cmp        w8,#0x4
      71006c2c48 03 0a 00 54     b.cc       LAB_71006c2d88
      71006c2c4c e8 03 1f 2a     mov        w8,wzr
      71006c2c50 bf 3b 03 d5     dmb        ISH
      71006c2c54 34 00 80 52     mov        w20,#0x1
      71006c2c58 74 fe e8 88     casal      w8,w20,[x19]
      71006c2c5c 1f 01 00 71     cmp        w8,#0x0
      71006c2c60 21 04 00 54     b.ne       LAB_71006c2ce4
                             LAB_71006c2c64                                  XREF[1]:     71006c2cfc(j)  
      71006c2c64 bf 3b 03 d5     dmb        ISH
      71006c2c68 e9 e7 02 b2     orr        x9,xzr,#-0x3333333333333334
      71006c2c6c 4a 01 80 52     mov        w10,#0xa
      71006c2c70 68 12 40 f9     ldr        x8,[x19, #0x20]
      71006c2c74 a9 99 99 f2     movk       x9,#0xcccd
      71006c2c78 09 7d c9 9b     umulh      x9,x8,x9
      71006c2c7c 29 fd 43 d3     lsr        x9,x9,#0x3
      71006c2c80 29 a1 0a 9b     msub       x9,x9,x10,x8
      71006c2c84 6a 0e 09 8b     add        x10,x19,x9, LSL #0x3
      71006c2c88 56 19 00 f9     str        x22,[x10, #0x30]
      71006c2c8c 6a 0a 40 f9     ldr        x10,[x19, #0x10]
      71006c2c90 df 02 0a eb     cmp        x22,x10
      71006c2c94 49 00 00 54     b.ls       LAB_71006c2c9c
      71006c2c98 76 0a 00 f9     str        x22,[x19, #0x10]
                             LAB_71006c2c9c                                  XREF[1]:     71006c2c94(j)  
      71006c2c9c 6a 0e 40 f9     ldr        x10,[x19, #0x18]
      71006c2ca0 df 02 0a eb     cmp        x22,x10
      71006c2ca4 42 00 00 54     b.cs       LAB_71006c2cac
      71006c2ca8 76 0e 00 f9     str        x22,[x19, #0x18]
                             LAB_71006c2cac                                  XREF[1]:     71006c2ca4(j)  
      71006c2cac b5 03 5d f8     ldur       x21,[x29, #local_90]
      71006c2cb0 08 05 00 91     add        x8,x8,#0x1
      71006c2cb4 68 12 00 f9     str        x8,[x19, #0x20]
      71006c2cb8 09 06 00 b5     cbnz       x9,LAB_71006c2d78
      71006c2cbc 1f 25 00 f1     cmp        x8,#0x9
      71006c2cc0 a8 02 00 54     b.hi       LAB_71006c2d14
      71006c2cc4 08 03 00 34     cbz        w8,LAB_71006c2d24
      71006c2cc8 09 01 40 92     and        x9,x8,#0x1
      71006c2ccc 1f 05 00 71     cmp        w8,#0x1
      71006c2cd0 e1 02 00 54     b.ne       LAB_71006c2d2c
      71006c2cd4 eb 03 1f aa     mov        x11,xzr
      71006c2cd8 ea 03 1f aa     mov        x10,xzr
      71006c2cdc 28 00 80 52     mov        w8,#0x1
      71006c2ce0 1e 00 00 14     b          LAB_71006c2d58
                             LAB_71006c2ce4                                  XREF[1]:     71006c2c60(j)  
      71006c2ce4 35 00 80 52     mov        w21,#0x1
      71006c2ce8 06 00 00 14     b          LAB_71006c2d00
                             LAB_71006c2cec                                  XREF[2]:     71006c2d04(j), 71006c2d10(j)  
      71006c2cec e8 03 1f 2a     mov        w8,wzr
      71006c2cf0 b5 06 00 11     add        w21,w21,#0x1
      71006c2cf4 74 fe e8 88     casal      w8,w20,[x19]
      71006c2cf8 1f 01 00 71     cmp        w8,#0x0
      71006c2cfc 40 fb ff 54     b.eq       LAB_71006c2c64
                             LAB_71006c2d00                                  XREF[1]:     71006c2ce8(j)  
      71006c2d00 bf 2e 00 72     tst        w21,#0xfff
      71006c2d04 41 ff ff 54     b.ne       LAB_71006c2cec
      71006c2d08 00 6a 98 52     mov        param_1+0x4,#0xc350
      71006c2d0c 1d 4c 60 94     bl         <EXTERNAL>::nn::os::SleepThread                  undefined SleepThread(TimeSpan p
      71006c2d10 f7 ff ff 17     b          LAB_71006c2cec
                             LAB_71006c2d14                                  XREF[1]:     71006c2cc0(j)  
      71006c2d14 4b 01 80 52     mov        w11,#0xa
      71006c2d18 e9 03 1f aa     mov        x9,xzr
      71006c2d1c e8 03 0b 2a     mov        w8,w11
      71006c2d20 04 00 00 14     b          LAB_71006c2d30
                             LAB_71006c2d24                                  XREF[1]:     71006c2cc4(j)  
      71006c2d24 00 e4 00 2f     movi       d0,#0x0
      71006c2d28 13 00 00 14     b          LAB_71006c2d74
                             LAB_71006c2d2c                                  XREF[1]:     71006c2cd0(j)  
      71006c2d2c 0b 7d 40 92     and        x11,x8,#0xffffffff
                             LAB_71006c2d30                                  XREF[1]:     71006c2d20(j)  
      71006c2d30 6b 79 7f 92     and        x11,x11,#0xfffffffe
      71006c2d34 ea 03 1f aa     mov        x10,xzr
      71006c2d38 6c e2 00 91     add        x12,x19,#0x38
      71006c2d3c ed 03 0b aa     mov        x13,x11
                             LAB_71006c2d40                                  XREF[1]:     71006c2d54(j)  
      71006c2d40 8e 81 5f f8     ldur       x14,[x12, #-0x8]
      71006c2d44 ad 09 00 f1     subs       x13,x13,#0x2
      71006c2d48 ca 01 0a 8b     add        x10,x14,x10
      71006c2d4c 8e 05 41 f8     ldr        x14,[x12], #0x10
      71006c2d50 ca 01 0a 8b     add        x10,x14,x10
      71006c2d54 61 ff ff 54     b.ne       LAB_71006c2d40
                             LAB_71006c2d58                                  XREF[1]:     71006c2ce0(j)  
      71006c2d58 89 00 00 b4     cbz        x9,LAB_71006c2d68
      71006c2d5c 69 0e 0b 8b     add        x9,x19,x11, LSL #0x3
      71006c2d60 29 19 40 f9     ldr        x9,[x9, #0x30]
      71006c2d64 2a 01 0a 8b     add        x10,x9,x10
                             LAB_71006c2d68                                  XREF[1]:     71006c2d58(j)  
      71006c2d68 40 01 63 9e     ucvtf      d0,x10
      71006c2d6c 01 01 63 1e     ucvtf      d1,w8
      71006c2d70 00 18 61 1e     fdiv       d0,d0,d1
                             LAB_71006c2d74                                  XREF[1]:     71006c2d28(j)  
      71006c2d74 60 16 00 fd     str        d0,[x19, #0x28]
                             LAB_71006c2d78                                  XREF[1]:     71006c2cb8(j)  
      71006c2d78 bf 3b 03 d5     dmb        ISH
      71006c2d7c 28 00 80 52     mov        w8,#0x1
      71006c2d80 7f fe e8 88     casal      w8,wzr,[x19]
      71006c2d84 bf 3b 03 d5     dmb        ISH
                             LAB_71006c2d88                                  XREF[2]:     71006c2c40(j), 71006c2c48(j)  
      71006c2d88 bc 01 00 b4     cbz        x28,LAB_71006c2dbc
      71006c2d8c a8 83 5c f8     ldur       x8,[x29, #local_98]
      71006c2d90 08 a5 40 f9     ldr        x8,[x8, #0x148]
      71006c2d94 48 01 00 b4     cbz        x8,LAB_71006c2dbc
      71006c2d98 08 19 40 f9     ldr        x8,[x8, #0x30]
      71006c2d9c 09 0f 01 d0     adrp       x9,DAT_71028a4000                                = ??
      71006c2da0 29 21 0e 91     add        x9,x9,#0x388
      71006c2da4 e1 03 16 aa     mov        param_2,x22
      71006c2da8 1f 01 00 f1     cmp        x8,#0x0
      71006c2dac 20 01 88 9a     csel       param_1=>DAT_71028a4388,x9,x8,eq                 = ??
      71006c2db0 08 00 40 f9     ldr        x8,[param_1]=>DAT_71028a4388                     = ??
      71006c2db4 08 0d 40 f9     ldr        x8,[x8, #0x18]
      71006c2db8 00 01 3f d6     blr        x8
    ```

응, `FUN_71006c2580`에 제대로 도착했어. 보내준 코드에 우리가 보려던 **시간값 전달 지점**도 포함돼 있어.

```asm
71006c2b8c  ldr x8,[x9,#0x30]    ; 현재 객체 +0x30의 참조
71006c2b90  cbz x8,LAB_71006c2b9c
71006c2b94  ldr x8,[x8,#0x28]    ; 참조 대상의 실제 객체
71006c2b98  str s0,[x8,#0x20]    ; 선택한 시간값 저장
```

이 부분은 확인됐으니 더 복사하지 않아도 돼. 다만 이 코드는 해당 객체를 읽는 곳이지, 생성하거나 연결하는 곳은 아니야.

## 다음 작업: 함수 참조 목록 열기

내가 앞서 “생성자나 초기화 함수를 찾아 달라”고 한 건 중간 설명이 부족했어. 우선 참조 목록부터 같이 확인하자.

1. 가운데 Listing 클릭 → `G`
2. 함수 시작 주소 입력:

```text
71006c2580
```

3. 함수 이름 또는 시작 명령에서 우클릭.
4. `References → Show References to` 선택. 메뉴 표기가 `Find References to…`로 나올 수도 있어. 이 기능으로 해당 위치를 참조하는 목록을 열 수 있어.[^3_1]
5. 나온 목록의 주소와 참조 종류만 보내줘.

앞서 확인한 `71019dc1cc`의 직접 호출은 이미 알고 있어. 이번에는 다른 호출이나 함수 주소를 담은 데이터 참조가 있는지 확인하려는 거야. 함수 전체를 다시 보낼 필요는 없어.

<span style="display:none">[^3_10][^3_2][^3_3][^3_4][^3_5][^3_6][^3_7][^3_8][^3_9]</span>

<div align="center">⁂</div>

[^3_1]: https://www.ghidradocs.com/11.0.1_PUBLIC/help/Base/help/topics/LocationReferencesPlugin/Location_References.html

[^3_2]: https://seanthegeek.net/posts/ghidra-workflow/

[^3_3]: https://cyberguardforte.com/tools/ghidra-quick-reference.php

[^3_4]: https://hackblue.org/learn/ghidra-cross-references

[^3_5]: https://reverseengineering.stackexchange.com/questions/33329/find-all-calls-of-a-given-function-with-ghidra

[^3_6]: https://www.reddit.com/r/ghidra/comments/ll494d/how_to_find_references_to_functions_that_only/

[^3_7]: https://www.tifkin.com/ghidra_markdown_docs/Ghidra_12.1_PUBLIC/SymbolTablePlugin/symbol_references/

[^3_8]: https://ghidra.re/ghidra_docs/api/ghidra/program/model/listing/Function.html

[^3_9]: https://bmaupin.github.io/wiki/programming/tools/ghidra

[^3_10]: https://ghidra.re/ghidra_docs/GhidraClass/Beginner/Introduction_to_Ghidra_Student_Guide.html


---

# ```

71019dc228

b FUN_71006c2580
UNCONDITIONAL_CALL
710271a3d0

addr FUN_71006c2580
DATA

```

좋아. 참조는 두 개고, `71019dc228`은 이미 본 호출자야. 다음에는 **데이터 참조**인 `710271a3d0`을 확인하자.

## 지금 할 작업

1. 가운데 Listing 클릭 → `G`
1. 아래 주소 입력:
   ```text
   710271a3d0
```

1. 그 위치의 앞뒤 약 10줄을 복사해서 보내줘.
    - 오른쪽 Decompiler가 아니라 가운데 Listing 내용이면 돼.
    - 주소와 `addr FUN_...` 같은 항목이 보이도록 복사해 줘.

## 이 위치를 보는 이유

`710271a3d0`에는 `FUN_71006c2580`의 주소가 데이터로 들어 있어. 주변 항목을 보면 가상 함수 테이블이나 다른 함수 포인터 목록의 일부인지 판단할 단서가 생겨.

아직 이 주소를 가상 함수 테이블이라고 확정한 것은 아니야. 우선 주변 데이터만 확인하고, 거기서 객체 초기화 경로로 연결할 수 있는지 보자.

---

# ```

                             PTR_LAB_710271a320                              XREF[1]:     FUN_71006bf5c0:71006bf884(*)  
      710271a320 54 9f 01        addr       LAB_7100019f54
                 00 71 00 
                 00 00
      710271a328 ec 07 6c        addr       LAB_71006c07ec
                 00 71 00 
                 00 00
      710271a330 f0 07 6c        addr       LAB_71006c07f0
                 00 71 00 
                 00 00
      710271a338 f8 07 6c        addr       LAB_71006c07f8
                 00 71 00 
                 00 00
      710271a340 00 08 6c        addr       LAB_71006c0800
                 00 71 00 
                 00 00
      710271a348 08 08 6c        addr       LAB_71006c0808
                 00 71 00 
                 00 00
      710271a350 0c 08 6c        addr       LAB_71006c080c
                 00 71 00 
                 00 00
      710271a358 18 08 6c        addr       LAB_71006c0818
                 00 71 00 
                 00 00
      710271a360 24 08 6c        addr       LAB_71006c0824
                 00 71 00 
                 00 00
      710271a368 2c 08 6c        addr       LAB_71006c082c
                 00 71 00 
                 00 00
      710271a370 30 08 6c        addr       LAB_71006c0830
                 00 71 00 
                 00 00
      710271a378 34 08 6c        addr       LAB_71006c0834
                 00 71 00 
                 00 00
      710271a380 00              ??         00h
      710271a381 00              ??         00h
      710271a382 00              ??         00h
      710271a383 00              ??         00h
      710271a384 00              ??         00h
      710271a385 00              ??         00h
      710271a386 00              ??         00h
      710271a387 00              ??         00h
      710271a388 00              ??         00h
      710271a389 00              ??         00h
      710271a38a 00              ??         00h
      710271a38b 00              ??         00h
      710271a38c 00              ??         00h
      710271a38d 00              ??         00h
      710271a38e 00              ??         00h
      710271a38f 00              ??         00h
                             PTR_LAB_710271a390                              XREF[1]:     FUN_71006c1ab8:71006c1ae8(*)  
      710271a390 64 23 6c        addr       LAB_71006c2364
                 00 71 00 
                 00 00
      710271a398 f0 1a 6c        addr       LAB_71006c1af0
                 00 71 00 
                 00 00
      710271a3a0 00 1b 6c        addr       FUN_71006c1b00
                 00 71 00 
                 00 00
      710271a3a8 d8 22 6c        addr       FUN_71006c22d8
                 00 71 00 
                 00 00
      710271a3b0 00              ??         00h
      710271a3b1 00              ??         00h
      710271a3b2 00              ??         00h
      710271a3b3 00              ??         00h
      710271a3b4 00              ??         00h
      710271a3b5 00              ??         00h
      710271a3b6 00              ??         00h
      710271a3b7 00              ??         00h
      710271a3b8 00              ??         00h
      710271a3b9 00              ??         00h
      710271a3ba 00              ??         00h
      710271a3bb 00              ??         00h
      710271a3bc 00              ??         00h
      710271a3bd 00              ??         00h
      710271a3be 00              ??         00h
      710271a3bf 00              ??         00h
      710271a3c0 84 86 6c        addr       FUN_71006c8684
                 00 71 00 
                 00 00
      710271a3c8 6c 25 6c        addr       LAB_71006c256c
                 00 71 00 
                 00 00
      710271a3d0 80 25 6c        addr       FUN_71006c2580
                 00 71 00 
                 00 00
      710271a3d8 20 3a 6c        addr       FUN_71006c3a20
                 00 71 00 
                 00 00
      710271a3e0 e0 3d 6c        addr       FUN_71006c3de0
                 00 71 00 
                 00 00
      710271a3e8 00              ??         00h
      710271a3e9 00              ??         00h
      710271a3ea 00              ??         00h
      710271a3eb 00              ??         00h
      710271a3ec 00              ??         00h
      710271a3ed 00              ??         00h
      710271a3ee 00              ??         00h
      710271a3ef 00              ??         00h
      710271a3f0 00              ??         00h
      710271a3f1 00              ??         00h
      710271a3f2 00              ??         00h
      710271a3f3 00              ??         00h
      710271a3f4 00              ??         00h
      710271a3f5 00              ??         00h
      710271a3f6 00              ??         00h
      710271a3f7 00              ??         00h
                             PTR_LAB_710271a3f8                              XREF[2]:     FUN_71006c6580:71006c6f88(*), 
                                                                                          FUN_71006c6580:71006c70b8(*)  
      710271a3f8 ac 85 6c        addr       LAB_71006c85ac
                 00 71 00 
                 00 00
      710271a400 90 84 6c        addr       LAB_71006c8490
                 00 71 00 
                 00 00
      710271a408 a0 84 6c        addr       FUN_71006c84a0
                 00 71 00 
                 00 00
      710271a410 00              ??         00h
      710271a411 00              ??         00h
      710271a412 00              ??         00h
      710271a413 00              ??         00h
      710271a414 00              ??         00h
      710271a415 00              ??         00h
      710271a416 00              ??         00h
      710271a417 00              ??         00h
      710271a418 00              ??         00h
      710271a419 00              ??         00h
      710271a41a 00              ??         00h
      710271a41b 00              ??         00h
      710271a41c 00              ??         00h
      710271a41d 00              ??         00h
      710271a41e 00              ??         00h
      710271a41f 00              ??         00h
    ```

좋아. 이 목록에서 `FUN_71006c2580`이 들어 있는 **함수 포인터 묶음**을 확인했어. 다만 아직 가상 함수 테이블이라고 확정할 수는 없어.

## 지금 보이는 관계

```text
710271a390  → LAB_71006c2364
710271a398  → LAB_71006c1af0
710271a3a0  → FUN_71006c1b00
710271a3a8  → FUN_71006c22d8
             ...
710271a3c0  → FUN_71006c8684
710271a3c8  → LAB_71006c256c
710271a3d0  → FUN_71006c2580
710271a3d8  → FUN_71006c3a20
710271a3e0  → FUN_71006c3de0
```

그리고 `710271a390`에는 다음 참조가 표시돼 있어.

```text
FUN_71006c1ab8 : 71006c1ae8
```

이 참조가 테이블 주소를 객체에 저장하는 코드인지 확인하면, 초기화 경로로 연결할 단서가 생겨. 중간의 0 값들만으로 테이블 경계를 정하지는 않을게.

## 다음 작업

1. 가운데 Listing 클릭 → `G`
2. 아래 주소 입력:

```text
71006c1ab8
```

3. 오른쪽 Decompiler의 함수 전체를 보내줘.

이번에는 그 함수가 짧은 초기화 함수인지부터 확인하고, `710271a390`을 어디에 저장하는지 볼 거야. 지금 목표는 여전히 `FUN_71006c2580`의 현재 객체에서 `+0x30` 필드가 어떻게 연결되는지 찾는 것이고, 패치 작업은 아니야.

---

# ```

void FUN_71006c1ab8(undefined8 *param_1,undefined8 param_2,undefined8 param_3,undefined8 *param_4)

{
undefined4 uVar1;
undefined4 uVar2;
undefined8 uVar3;
undefined8 uVar4;

*param_1 = \&PTR_LAB_710271a5e0;
uVar1 = *(undefined4 *)(param_4 + 1);
uVar2 = *(undefined4 *)((long)param_4 + 0xc);
uVar3 = *param_4;
uVar4 = param_4[2];
param_1[5] = param_3;
*(undefined4 *)(param_1 + 6) = 0;
*(undefined4 *)(param_1 + 2) = uVar1;
*(undefined4 *)((long)param_1 + 0x14) = uVar2;
param_1[3] = uVar4;
param_1[4] = param_2;
*param_1 = \&PTR_LAB_710271a390;
param_1[1] = uVar3;
return;
}

```

확인했어. 이 함수는 객체의 함수 테이블 포인터와 초기 필드를 설정하는 **초기화 함수**야. 다만 우리가 찾는 `+0x30`의 객체 연결은 여기서 나오지 않았어.

## 이번 코드에서 확인된 것

마지막에 객체 시작 위치에 다음 주소를 넣어.

```c
*param_1 = &PTR_LAB_710271a390;
```

따라서 앞서 본 함수 포인터 묶음이 이 객체의 함수 테이블로 쓰인다는 연결은 확인됐어.

반면 `+0x30`에는 포인터를 넣는 게 아니라 4바이트를 0으로 초기화해.

```c
*(undefined4 *)(param_1 + 6) = 0;  // 객체 +0x30
```

여기서 주의해야 해. 이 초기화 함수의 `param_1`과 `FUN_71006c2580`의 `param_1`이 같은 객체 시작점을 가리킨다고 아직 확인하지 않았어. 따라서 두 함수의 `+0x30`을 곧바로 같은 필드라고 묶으면 안 돼.

## 다음은 `FUN_71006c1b00` 확인

앞서 테이블에 함께 있던 이 함수를 보자.

```text
71006c1b00
```

1. 가운데 Listing 클릭 → `G`
2. 위 주소 입력.
3. 오른쪽 Decompiler 함수 전체를 보내줘.

현재 함수에서 바로 가까운 다음 함수이고, 같은 테이블에도 등록돼 있어. 그 본문에서 객체를 추가로 초기화하거나 다른 객체에 연결하는지 확인하자. 아직 코드는 수정하지 말자.

---

# ```

void FUN_71006c1b00(long param_1,long param_2)

{
bool bVar1;
uint uVar2;
int iVar3;
long lVar4;
int iVar5;
undefined8 uVar6;
long *plVar7;
float *pfVar8;
float fVar9;
float fVar10;
float fVar11;
float fVar12;
float fVar13;
float fVar14;
float fVar15;
float fVar16;
float fVar17;
float fVar18;
float fVar19;
float fVar20;
float local_b0;
float fStack_ac;
float local_a8;
undefined4 local_a4;
float local_a0 [2];
ulong local_98;
float local_90;
float fStack_8c;
float local_88;
float local_84;
float local_74;

lVar4 = *(long *)(*(long *)(param_1 + 0x20) + 0x28);
if (*(int *)(lVar4 + 8) == 0) {
return;
}
lVar4 = **(long **)(lVar4 + 0x38);
if (lVar4 == 0) {
return;
}
uVar6 = *(undefined8 *)(*(long *)(param_2 + 8) + 8);
plVar7 = *(long **)(*(long *)(param_1 + 0x28) + 0x28);
bVar1 = FUN_710055543c(lVar4,8);
uVar2 = *(uint *)(param_1 + 0x30);
if (bVar1) {
uVar2 = uVar2 ^ 1;
*(uint *)(param_1 + 0x30) = uVar2;
}
if ((uVar2 \& 1) != 0) {
return;
}
if ((((((*(char *)(lVar4 + 0x16) == '\0') || ((*(byte *)(lVar4 + 0x70) \& 1) == 0)) \&\&
((uVar2 = *(uint *)(lVar4 + 0x20), (uVar2 \& 1) == 0 || ((*(byte *)(lVar4 + 0x1b0) \& 1) == 0)
))) \&\& (((uVar2 >> 2 \& 1) == 0 || ((*(byte *)(lVar4 + 0xc0) \& 1) == 0)))) \&\&
(((uVar2 >> 3 \& 1) == 0 || ((*(byte *)(lVar4 + 0x110) \& 1) == 0)))) \&\&
((fVar12 = 0.0, (uVar2 >> 4 \& 1) == 0 || ((*(byte *)(lVar4 + 0x160) \& 1) == 0)))) {
fVar11 = -0.0;
if (*(char *)(lVar4 + 0x16) == '\0') goto LAB_71006c1c50;
LAB_71006c1c48:
if ((*(byte *)(lVar4 + 0x70) \& 1) == 0) goto LAB_71006c1c50;
LAB_71006c1c94:
FUN_7101e4a538(lVar4,\&local_90,local_a0);
iVar3 = (int)(local_90 * 128.0);
iVar5 = (int)(local_a0[0] * 128.0);
if (iVar3 < -0x7f) {
iVar3 = -0x80;
}
if (0x7e < iVar3) {
iVar3 = 0x7f;
}
if (iVar5 < -0x7f) {
iVar5 = -0x80;
}
if (0x7e < iVar5) {
iVar5 = 0x7f;
}
fVar9 = (float)iVar3 * -0.002;
fVar20 = (float)iVar5 * 0.002;
}
else {
FUN_7101e4ac14(lVar4,\&local_90,local_a0);
iVar3 = (int)(local_90 * 128.0);
iVar5 = (int)(local_a0[0] * 128.0);
if (iVar3 < -0x7f) {
iVar3 = -0x80;
}
if (0x7e < iVar3) {
iVar3 = 0x7f;
}
if (iVar5 < -0x7f) {
iVar5 = -0x80;
}
if (0x7e < iVar5) {
iVar5 = 0x7f;
}
fVar11 = (float)iVar3 * -0.017453292;
fVar12 = (float)iVar5 * 0.017453292;
if (*(char *)(lVar4 + 0x16) != '\0') goto LAB_71006c1c48;
LAB_71006c1c50:
uVar2 = *(uint *)(lVar4 + 0x20);
if (((((uVar2 \& 1) != 0) \&\& ((*(byte *)(lVar4 + 0x1b0) \& 1) != 0)) ||
(((uVar2 >> 2 \& 1) != 0 \&\& ((*(byte *)(lVar4 + 0xc0) \& 1) != 0)))) ||
((((uVar2 >> 3 \& 1) != 0 \&\& ((*(byte *)(lVar4 + 0x110) \& 1) != 0)) ||
((fVar20 = 0.0, (uVar2 >> 4 \& 1) != 0 \&\& ((*(byte *)(lVar4 + 0x160) \& 1) != 0))))))
goto LAB_71006c1c94;
fVar9 = -0.0;
}
(**(code **)(*plVar7 + 0x98))(plVar7,\&local_90);
fVar14 = fStack_8c + fStack_8c;
fVar10 = asinf(fVar14 * local_88 - (local_90 + local_90) * local_84);
fVar14 = atan2f(local_90 * (local_88 + local_88) + fVar14 * local_84,
(1.0 - local_90 * (local_90 + local_90)) - fStack_8c * fVar14);
local_a0[0] = 0.0;
local_a0[1] = 0.0;
local_98 = 0;
for (fVar14 = fVar14 + fVar11 * 0.01; 6.2831855 < fVar14; fVar14 = fVar14 + -6.2831855) {
}
fVar10 = fVar10 + fVar12 * 0.01;
for (; fVar14 < -6.2831855; fVar14 = fVar14 + 6.2831855) {
}
for (; 6.2831855 < fVar10; fVar10 = fVar10 + -6.2831855) {
}
for (; fVar10 < -6.2831855; fVar10 = fVar10 + 6.2831855) {
}
local_b0 = 0.0;
FUN_71011568c0(fVar14,local_a0,(float *)((ulong)local_a0 | 8));
pfVar8 = (float *)((ulong)local_a0 | 4);
FUN_71011568c0(fVar10,pfVar8,\&local_b0);
local_74 = 0.0;
fVar14 = local_b0 * local_a0[0];
fVar11 = local_b0 * (float)local_98;
fVar15 = 1.0 / SQRT(local_a0[1] * local_a0[1] + fVar14 * fVar14 + fVar11 * fVar11);
fVar11 = fVar11 * fVar15;
fVar14 = fVar14 * fVar15;
fVar15 = local_a0[1] * fVar15;
FUN_71011568c0(fVar10 + 1.5707964,pfVar8,\&local_74);
local_a0[0] = local_74 * local_a0[0];
local_98._0_4_ = local_74 * (float)local_98;
fVar10 = 1.0 / SQRT(local_a0[1] * local_a0[1] + local_a0[0] * local_a0[0] +
(float)local_98 * (float)local_98);
fVar16 = -((float)local_98 * fVar10 * fVar15) + fVar11 * local_a0[1] * fVar10;
fVar12 = -(local_a0[0] * fVar10 * fVar11) + fVar14 * (float)local_98 * fVar10;
fVar10 = -(local_a0[1] * fVar10 * fVar14) + fVar15 * local_a0[0] * fVar10;
fVar13 = 1.0 / SQRT(fVar12 * fVar12 + fVar16 * fVar16 + fVar10 * fVar10);
local_a0[0] = -(fVar11 * fVar12 * fVar13) + fVar10 * fVar13 * fVar15;
local_a0[1] = -(fVar14 * fVar10 * fVar13) + fVar16 * fVar13 * fVar11;
fVar12 = -(fVar15 * fVar16 * fVar13) + fVar12 * fVar13 * fVar14;
fVar10 = 1.0 / SQRT(local_a0[1] * local_a0[1] + local_a0[0] * local_a0[0] + fVar12 * fVar12);
local_a0[0] = local_a0[0] * fVar10;
local_a0[1] = local_a0[1] * fVar10;
fVar12 = fVar12 * fVar10;
local_98 = (ulong)(uint)fVar12;
fVar17 = -(fVar12 * fVar15) + fVar11 * local_a0[1];
fVar16 = -(local_a0[0] * fVar11) + fVar14 * fVar12;
fVar10 = -(local_a0[1] * fVar14) + fVar15 * local_a0[0];
fVar12 = 1.0 / SQRT(fVar15 * fVar15 + fVar14 * fVar14 + fVar11 * fVar11);
fVar13 = 1.0 / SQRT(fVar16 * fVar16 + fVar17 * fVar17 + fVar10 * fVar10);
fVar14 = fVar14 * fVar12;
fVar15 = fVar15 * fVar12;
fVar11 = fVar11 * fVar12;
fVar17 = fVar17 * fVar13;
fVar16 = fVar16 * fVar13;
fVar10 = fVar10 * fVar13;
fVar18 = -(fVar14 * fVar10) + fVar17 * fVar11;
fVar12 = -(fVar11 * fVar16) + fVar10 * fVar15;
fVar13 = -(fVar15 * fVar17) + fVar16 * fVar14;
fVar19 = fVar11 + fVar17 + fVar18;
if (0.0 < fVar19) {
fVar19 = fVar19 + 1.0;
fVar11 = SQRT(fVar19);
if (NAN(fVar11)) {
fVar11 = sqrtf(fVar19);
}
local_84 = (1.0 / fVar11) * 0.5;
local_90 = (fVar13 - fVar15) * local_84;
fStack_8c = (fVar14 - fVar10) * local_84;
local_88 = (fVar16 - fVar12) * local_84;
local_84 = fVar19 * local_84;
goto LAB_71006c2164;
}
if (fVar18 <= fVar17) {
if (fVar17 < fVar11) goto LAB_71006c20d0;
fVar17 = (fVar17 - (fVar11 + fVar18)) + 1.0;
fVar11 = SQRT(fVar17);
if (NAN(fVar11)) {
fVar11 = sqrtf(fVar17);
}
fVar11 = (1.0 / fVar11) * 0.5;
local_90 = fVar17 * fVar11;
fStack_8c = (fVar16 + fVar12) * fVar11;
local_88 = (fVar14 + fVar10) * fVar11;
local_84 = fVar13 - fVar15;
}
else if (fVar18 < fVar11) {
LAB_71006c20d0:
fVar17 = (fVar11 - (fVar17 + fVar18)) + 1.0;
fVar11 = SQRT(fVar17);
if (NAN(fVar11)) {
fVar11 = sqrtf(fVar17);
}
local_84 = fVar16 - fVar12;
fVar11 = (1.0 / fVar11) * 0.5;
local_90 = (fVar14 + fVar10) * fVar11;
fStack_8c = (fVar15 + fVar13) * fVar11;
local_88 = fVar17 * fVar11;
}
else {
fVar17 = (fVar18 - (fVar11 + fVar17)) + 1.0;
fVar11 = SQRT(fVar17);
if (NAN(fVar11)) {
fVar11 = sqrtf(fVar17);
}
fVar11 = (1.0 / fVar11) * 0.5;
local_90 = (fVar16 + fVar12) * fVar11;
fStack_8c = fVar17 * fVar11;
local_88 = (fVar15 + fVar13) * fVar11;
local_84 = fVar14 - fVar10;
}
local_84 = local_84 * fVar11;
LAB_71006c2164:
(**(code **)(*plVar7 + 0x80))(plVar7,\&local_90);
local_a4 = 0;
fVar14 = local_88 * -0.0 + fVar20 * fStack_8c + fVar9 * local_84;
fVar10 = -(local_90 * fVar20) + fVar9 * local_88 + local_84 * 0.0;
fVar11 = fStack_8c * 0.0 + local_90 * fVar9 + local_88 * fVar20;
fVar12 = -(fStack_8c * fVar9) + local_90 * 0.0 + fVar20 * local_84;
local_b0 = -(local_88 * fVar10) + fVar12 * fStack_8c + local_90 * fVar11 + fVar14 * local_84;
fStack_ac = -(local_90 * fVar12) + fVar14 * local_88 + fStack_8c * fVar11 + fVar10 * local_84;
local_a8 = -(fStack_8c * fVar14) + fVar10 * local_90 + local_88 * fVar11 + fVar12 * local_84;
(**(code **)(*plVar7 + 0x28))(plVar7,\&local_b0);
(**(code **)(*plVar7 + 0x478))(plVar7,uVar6);
return;
}

```

이 함수는 우리가 찾던 객체 연결 초기화가 아니야. 보내준 본문에서는 입력을 읽고 방향·위치를 계산하는 **조작 처리**가 보여. 따라서 이 가지는 여기서 멈추는 게 맞아.

## 이번 코드로 확인된 점

초기화 함수에서 0으로 설정했던 `+0x30`은 여기서 토글 플래그로 사용돼.

```c
bVar1 = FUN_710055543c(lVar4, 8);

if (bVar1) {
    uVar2 ^= 1;
    *(uint *)(param_1 + 0x30) = uVar2;
}

if ((uVar2 & 1) != 0) {
    return;
}
```

그 뒤에는 앞서 확인한 스틱 읽기 함수들이 호출되고, 삼각함수와 방향 계산을 거쳐 다른 객체에 결과를 전달해.

```c
FUN_7101e4ac14(...);
FUN_7101e4a538(...);

/* 방향·위치 계산 */

가상함수_0x80(plVar7, &local_90);
가상함수_0x28(plVar7, &local_b0);
```

카메라나 자유 이동 조작일 가능성은 있지만, 정확한 대상은 아직 확정하지 않을게.

중요한 것은 이 객체의 `+0x30`이 우리가 찾는 시간 전달용 참조 필드가 아니라는 점이야. 앞서 내가 인접한 테이블 항목을 따라가도록 해서 우회했어.

## 다음은 테이블에서 직접 연결된 함수로 돌아가자

우리가 추적하던 함수 주소는 다음 위치에 있었어.

```text
710271a3d0 → FUN_71006c2580
```

그 바로 앞의 함수 포인터는 다음이었지.

```text
710271a3c0 → FUN_71006c8684
```

이 함수가 생성·해제·다른 처리 중 무엇인지 본문으로 확인하자. 가까운 주소라는 이유만으로 초기화 함수라고 가정하지는 않을게.

1. 가운데 Listing 클릭 → `G`
2. 아래 주소 입력:

```text
71006c8684
```

3. 오른쪽 Decompiler의 함수 전체를 보내줘.

이번에는 함수가 짧다면 그 내용만으로 테이블의 역할을 구분할 수 있어. 아직 패치나 필드 이름 변경은 하지 말자.

---

# ```

void FUN_71006c8684(undefined8 *param_1)

{
*param_1 = \&PTR_LAB_710271a5e0;
return;
}

```

이 함수는 **테이블 포인터 재설정**만 해. 객체의 첫 8바이트를 다음 주소로 바꾸고 끝나.

```c
*param_1 = &PTR_LAB_710271a5e0;
```

이 본문만으로 생성자·소멸자 중 어느 쪽인지 확정할 수 없고, 우리가 찾는 `+0x30` 필드의 연결 정보도 없어. 내가 테이블의 인접 함수를 차례로 열도록 한 건 비효율적이었어.

## 다음은 테이블 주소 자체의 참조 확인

이번에는 `FUN_71006c2580`이 들어 있는 묶음의 후보 시작 주소를 누가 사용하는지 보자.

```text
710271a3c0
```

1. 가운데 Listing 클릭 → `G` → `710271a3c0` 입력.
2. 해당 데이터 항목에서 우클릭 → `References → Show References to`.
3. 나오는 참조 목록의 주소와 종류만 보내줘.
    - 참조가 없으면 “참조 없음”이라고 알려줘.
    - 아직 함수 전체를 복사할 필요는 없어.

`710271a390`이 아니라 `710271a3c0`이야. 앞서 `710271a390`을 따라간 경로는 입력 조작 객체로 이어졌으므로, 이번에는 두 묶음을 구분해서 확인하려는 거야.

---

# \<q>

> 참조 없음

\</q>

알겠어. **참조 없음**으로 기록하고, 이 테이블 쪽 추적은 일단 멈추자. 인접 함수만 계속 열도록 해서 돌아갔어.

## 다음은 함수의 맨 앞부분만 확인하자

앞서 보낸 Listing은 `71006c25f8`부터 시작했어. 그보다 앞에 있는 함수 진입부가 필요해.

1. 가운데 Listing 클릭 → `G`
2. 아래 주소 입력:

```text
71006c2580
```

3. 아래 범위만 복사해서 보내줘.

```text
71006c2580 ~ 71006c25f8
```


약 30줄 정도이고, 오른쪽 Decompiler가 아니라 가운데 Listing이면 돼.

## 확인하려는 것

앞서 시간값을 전달하는 명령은 이거였어.

```asm
71006c2b8c  ldr x8,[x9,#0x30]
71006c2b94  ldr x8,[x8,#0x28]
71006c2b98  str s0,[x8,#0x20]
```

이번에는 함수 시작에서 원래 첫 번째 인자가 어느 레지스터에 보관되는지 확인해서, 여기서 사용하는 객체 기준점을 정확히 연결할 거야. 함수 전체나 오디오 코드는 다시 보낼 필요 없어.

---

# ```

                             **************************************************************
                             *                          FUNCTION                          *
                             **************************************************************
                               undefined __cdecl FUN_71006c2580(long * param_1, ulong p
             undefined         <UNASSIGNED>   <RETURN>
             long *            x0:8           param_1
             ulong             x1:8           param_2
             undefined8        x2:8           param_3
             long *            x3:8           param_4
             undefined8        Stack[-0x10]:8 local_10                                XREF[2]:     71006c259c(W), 
                                                                                                   71006c39a0(R)  
             undefined8        Stack[-0x20]:8 local_20                                XREF[2]:     71006c2598(W), 
                                                                                                   71006c39a4(R)  
             undefined8        Stack[-0x30]:8 local_30                                XREF[2]:     71006c2594(W), 
                                                                                                   71006c39a8(R)  
             undefined8        Stack[-0x40]:8 local_40                                XREF[2]:     71006c2590(W), 
                                                                                                   71006c39ac(R)  
             undefined8        Stack[-0x50]:8 local_50                                XREF[2]:     71006c258c(W), 
                                                                                                   71006c39b0(R)  
             undefined8        Stack[-0x60]:8 local_60                                XREF[2]:     71006c2588(W), 
                                                                                                   71006c39b4(*)  
             undefined4        Stack[-0x64]:4 local_64                                XREF[2]:     71006c3660(W), 
                                                                                                   71006c3754(W)  
             undefined4        Stack[-0x68]:4 local_68                                XREF[3]:     71006c3658(W), 
                                                                                                   71006c374c(W), 
                                                                                                   71006c37d8(W)  
             undefined8        Stack[-0x70]:8 local_70                                XREF[7]:     71006c28bc(W), 
                                                                                                   71006c28fc(W), 
                                                                                                   71006c3164(R), 
                                                                                                   71006c3648(W), 
                                                                                                   71006c3740(W), 
                                                                                                   71006c37d0(W), 
                                                                                                   71006c3964(W)  
             undefined4        Stack[-0x74]:4 local_74                                XREF[1]:     71006c3174(R)  
             undefined4        Stack[-0x78]:4 local_78                                XREF[1]:     71006c3180(R)  
             undefined4        Stack[-0x7c]:4 local_7c                                XREF[1]:     71006c318c(R)  
             undefined8        Stack[-0x88]:8 local_88                                XREF[2]:     71006c2894(W), 
                                                                                                   71006c2b20(R)  
             undefined8        Stack[-0x90]:8 local_90                                XREF[5]:     71006c2858(W), 
                                                                                                   71006c2a54(R), 
                                                                                                   71006c2ae4(R), 
                                                                                                   71006c2cac(R), 
                                                                                                   71006c3764(R)  
             undefined8        Stack[-0x98]:8 local_98                                XREF[3]:     71006c2824(W), 
                                                                                                   71006c2c08(R), 
                                                                                                   71006c2d8c(R)  
             undefined8        Stack[-0xa0]:8 local_a0                                XREF[5]:     71006c285c(W), 
                                                                                                   71006c2b34(R), 
                                                                                                   71006c31c0(R), 
                                                                                                   71006c33b8(R), 
                                                                                                   71006c39c4(R)  
             undefined4        Stack[-0xa4]:4 local_a4                                XREF[4]:     71006c2614(W), 
                                                                                                   71006c26dc(W), 
                                                                                                   71006c2b9c(R), 
                                                                                                   71006c394c(R)  
             undefined8        Stack[-0xb0]:8 local_b0                                XREF[3]:     71006c2820(W), 
                                                                                                   71006c2bac(R), 
                                                                                                   71006c3948(R)  
             undefined8        Stack[-0xc0]:8 local_c0                                XREF[3]:     71006c28a4(W), 
                                                                                                   71006c2a58(R), 
                                                                                                   71006c2ae8(R)  
             undefined8        Stack[-0xc8]:8 local_c8                                XREF[1]:     71006c286c(W)  
             undefined8        Stack[-0xd0]:8 local_d0                                XREF[2]:     71006c2860(W), 
                                                                                                   71006c2b28(R)  
             undefined8        Stack[-0xd8]:8 local_d8                                XREF[2]:     71006c26f4(W), 
                                                                                                   71006c2b3c(R)  
             undefined8        Stack[-0xe0]:8 local_e0                                XREF[2]:     71006c25fc(W), 
                                                                                                   71006c2b88(R)  
             undefined8        Stack[-0xe8]:8 local_e8                                XREF[4]:     71006c2868(W), 
                                                                                                   71006c31b4(R), 
                                                                                                   71006c33b4(R), 
                                                                                                   71006c39c0(R)  
             undefined8        Stack[-0xf0]:8 local_f0                                XREF[2]:     71006c2708(W), 
                                                                                                   71006c38a4(R)  
             undefined4        Stack[-0xf4]:4 local_f4                                XREF[2]:     71006c2730(W), 
                                                                                                   71006c2b58(R)  
             undefined8        Stack[-0x100   local_100                               XREF[3]:     71006c2864(W), 
                                                                                                   71006c2b74(R), 
                                                                                                   71006c37d4(R)  
                             FUN_71006c2580                                  XREF[2]:     FUN_71019dc1cc:71019dc228(c), 
                                                                                          710271a3d0(*)  
      71006c2580 7f 23 03 d5     pacibsp
      71006c2584 ff 03 04 d1     sub        sp,sp,#0x100
      71006c2588 fd 7b 0a a9     stp        x29,x30,[sp, #local_60]
      71006c258c fc 6f 0b a9     stp        x28,x27,[sp, #local_50]
      71006c2590 fa 67 0c a9     stp        x26,x25,[sp, #local_40]
      71006c2594 f8 5f 0d a9     stp        x24,x23,[sp, #local_30]
      71006c2598 f6 57 0e a9     stp        x22,x21,[sp, #local_20]
      71006c259c f4 4f 0f a9     stp        x20,x19,[sp, #local_10]
      71006c25a0 fd 83 02 91     add        x29,sp,#0xa0
      71006c25a4 29 04 40 f9     ldr        x9,[param_2, #0x8]
      71006c25a8 f6 03 00 aa     mov        x22,param_1
      71006c25ac f5 03 01 aa     mov        x21,param_2
      71006c25b0 c8 8e 45 f8     ldr        x8,[x22, #0x58]!
      71006c25b4 f7 03 00 aa     mov        x23,param_1
      71006c25b8 39 05 40 f9     ldr        x25,[x9, #0x8]
      71006c25bc c9 82 5c f8     ldur       x9,[x22, #-0x38]
      71006c25c0 13 15 40 f9     ldr        x19,[x8, #0x28]
      71006c25c4 38 15 40 f9     ldr        x24,[x9, #0x28]
      71006c25c8 c9 06 40 f9     ldr        x9,[x22, #0x8]
      71006c25cc 68 5e 40 91     add        x8,x19,#0x17, LSL #12
      71006c25d0 3b 15 40 f9     ldr        x27,[x9, #0x28]
      71006c25d4 09 21 3b 91     add        x9,x8,#0xec8
      71006c25d8 68 b2 40 f9     ldr        x8,[x19, #0x160]
      71006c25dc e8 00 00 b4     cbz        x8,LAB_71006c25f8
      71006c25e0 00 15 40 f9     ldr        param_1,[x8, #0x28]
      71006c25e4 f4 03 09 aa     mov        x20,x9
      71006c25e8 08 00 40 f9     ldr        x8,[param_1]
      71006c25ec 08 cd 42 f9     ldr        x8,[x8, #0x598]
      71006c25f0 00 01 3f d6     blr        x8
      71006c25f4 e9 03 14 aa     mov        x9,x20
                             LAB_71006c25f8                                  XREF[1]:     71006c25dc(j)  
      71006c25f8 34 69 40 a9     ldp        x20,x26,[x9]
    ```

좋아. 이번 진입부로 **객체 기준점**이 확인됐어. 특히 `x22`는 중간에 값이 바뀌므로, 원래 객체 주소와 구분해야 해.

## 레지스터와 객체의 관계

원래 첫 번째 인자 주소를 `this`라고 쓰면 다음과 같아.

```asm
71006c25a8  mov x22,x0
71006c25b0  ldr x8,[x22,#0x58]!
71006c25b4  mov x23,x0
```

`71006c25b0`에는 끝에 `!`가 있어서, 데이터를 읽으면서 `x22`도 갱신해.

```text
x23 = this
x22 = this + 0x58
x19 = *( *(this + 0x58) + 0x28 )
x27 = *( *(this + 0x60) + 0x28 )
```

앞서 시간값을 전달하던 곳의 `x9`는 저장해 둔 `x23`, 즉 원래 `this`를 다시 읽은 값이야.

```asm
71006c285c  stur x23,[x29,#local_a0]
...
71006c2b34  ldur x9,[x29,#local_a0]
...
71006c2b8c  ldr x8,[x9,#0x30]
71006c2b94  ldr x8,[x8,#0x28]
71006c2b98  str s0,[x8,#0x20]
```

따라서 추적 대상은 정확히 다음이야.

```c
참조객체 = *(long *)(this + 0x30);
실제객체 = *(long *)(참조객체 + 0x28);
*(float *)(실제객체 + 0x20) = 선택한_시간값;
```


## 다음 작업: 원래 객체의 초기화 경로 찾기

이미 본 호출자 `FUN_71019dc1cc`는 첫 번째 인자를 바꾸지 않고 이 함수에 넘겼어. 따라서 이제 그 호출자의 참조를 확인하자. 앞서 잘못 따라간 `710271a390` 쪽은 더 볼 필요 없어.

1. 가운데 Listing 클릭 → `G`
2. 아래 주소 입력:

```text
71019dc1cc
```

3. 함수 이름에서 우클릭 → `References → Show References to`.
4. 나온 주소와 참조 종류만 보내줘.

이번에는 현재 객체를 받는 것으로 확인된 호출자에서 출발할 거야. 함수 전체는 다시 보내지 않아도 돼.

---

# 참조 없음

알겠어. `FUN_71019dc1cc`도 **참조 없음**으로 기록할게. 이 경로로는 객체 초기화 위치를 아직 연결하지 못했어.

내가 참조와 인접 함수를 번갈아 확인하게 해서 돌아갔어. 다음에는 이미 확보한 시간값 수신 객체 쪽으로 범위를 좁히자.

## 다음 작업 하나만

`FUN_71006c2580`에서 시간값을 저장하는 곳으로 이동해.

```text
71006c2b98
```

그 위치 자체는 이미 확인했으니 다시 복사하지 않아도 돼. 이제 오른쪽 Decompiler에서 다음 표현에 해당하는 변수를 확인해 줘.

```c
*(long *)(param_1[^11_6] + 0x28)
```

가능하면 이 표현의 `param_1[^11_6]`을 우클릭했을 때 나오는 메뉴를 캡처해 줘. 현재 Ghidra 화면에서 필드 사용처를 추적할 수 있는 기능이 제공되는지부터 확인하자.

단, 이 작업으로 함수 밖의 모든 `+0x30` 쓰기 위치가 자동으로 찾아진다고 보장할 수는 없어. 현재까지 확정한 것은 시간값의 저장 경로뿐이고, 객체 종류와 생성 위치는 아직 미확인이야.

<span style="display:none">[^11_1]</span>

<div align="center">⁂</div>

[^11_1]: https://github.com/zernanvash/cheatsheet/blob/main/reverse-engineering-vault/tools/Ghidra Cheat Sheet.md


---

# 캡쳐는 못해 그냥 요구해 reference 관련 옵션이 있네

좋아. 캡처 대신 **메뉴 이름**만 알려줘.

`param_1[^12_6]`에서 우클릭한 뒤, `References` 관련 하위 메뉴를 열어서 나오는 항목들을 그대로 적어줘. 회색으로 비활성화된 항목은 옆에 “비활성”이라고 표시해 줘.

아직 아무 항목도 실행하거나 코드를 수정할 필요는 없어. 메뉴를 확인한 뒤 정확히 하나를 지정할게.

<span style="display:none">[^12_1]</span>

<div align="center">⁂</div>

[^12_1]: https://github.com/zernanvash/cheatsheet/blob/main/reverse-engineering-vault/tools/Ghidra Cheat Sheet.md


---

# - find uses of long \*

- find reference to ...2b8c

그 두 옵션은 지금 찾으려는 `+0x30` 필드의 쓰기 위치를 직접 가리키는 항목은 아니야. **선택하지 말고** 메뉴를 닫아줘.

- `Find uses of long *`는 `long *` 타입의 사용처를 찾는 항목이야.
- `Find reference to …2b8c`는 지금 선택한 명령 주소 `71006c2b8c`의 참조를 찾는 항목이야.

내가 이 메뉴를 확인하게 한 건 적절한 다음 단계가 아니었어. 더 진행하기 전에 추적 방향을 정리할게.

## 지금 확정한 것과 막힌 것

확정한 경로:

```text
현재 객체 +0x30
  → 참조 객체 +0x28
  → 실제 객체 +0x20에 시간값 저장
```

아직 모르는 것:

```text
현재 객체 +0x30에 누가 참조 객체를 넣는가?
```

이것을 찾으려면 현재 객체의 초기화 위치나 실제 타입을 확인해야 하는데, 지금까지의 참조 목록만으로는 연결하지 못했어. 같은 메뉴와 인접 함수를 계속 확인하게 하지는 않을게.

## 다음에 필요한 최소 정보

가운데 Listing에서 아래 주소로 이동해 줘.

```text
71019dc1cc
```

그리고 함수 시작 바로 위에 있는 라벨·데이터·명령을 포함해 약 15줄만 복사해서 보내줘. 오른쪽 Decompiler 내용은 이미 있으니 필요 없어.

이 함수가 독립 함수로 배치됐는지, 주변에 다른 진입점이나 테이블 관련 단서가 있는지 먼저 확인하자. 단서가 없으면 이 정적 추적 경로가 막혔다고 명확히 정리할게.

---

# ```

                             LAB_71019dc198                                  XREF[1]:     71019dc110(j)  
      71019dc198 e0 03 13 aa     mov        param_1,x19
      71019dc19c e1 03 1f 2a     mov        param_2+0x4,wzr
      71019dc1a0 02 00 80 12     mov        param_3+0x4,#0xffffffff
      71019dc1a4 66 67 c6 97     bl         FUN_7100b75f3c                                   undefined8 FUN_7100b75f3c(long p
      71019dc1a8 1f 1c 00 72     tst        param_1+0x4,#0xff
      71019dc1ac 40 fb ff 54     b.eq       LAB_71019dc114
      71019dc1b0 e0 03 13 aa     mov        param_1,x19
      71019dc1b4 63 65 c6 97     bl         FUN_7100b75740                                   undefined8 FUN_7100b75740(long p
      71019dc1b8 e0 03 14 aa     mov        param_1,x20
      71019dc1bc e1 03 13 aa     mov        param_2,x19
      71019dc1c0 c8 d2 ab 97     bl         FUN_71004d0ce0                                   undefined FUN_71004d0ce0(long pa
      71019dc1c4 55 00 80 52     mov        w21,#0x2
      71019dc1c8 eb ff ff 17     b          LAB_71019dc174
                             **************************************************************
                             *                          FUNCTION                          *
                             **************************************************************
                               undefined __cdecl FUN_71019dc1cc(long * param_1, ulong p
             undefined         <UNASSIGNED>   <RETURN>
             long *            x0:8           param_1
             ulong             x1:8           param_2
             undefined8        x2:8           param_3
             undefined8        Stack[-0x10]:8 local_10                                XREF[2]:     71019dc1d4(W), 
                                                                                                   71019dc21c(R)  
             undefined8        Stack[-0x20]:8 local_20                                XREF[2]:     71019dc1d0(W), 
                                                                                                   71019dc220(*)  
                             FUN_71019dc1cc
      71019dc1cc 7f 23 03 d5     pacibsp
    ```

확인했어. 보내준 범위에서는 `71019dc1c8`의 분기가 앞쪽 `71019dc174`로 돌아가고, 그 다음 `71019dc1cc`에서 별도 함수가 시작해. 이 주변에는 우리가 찾는 객체 초기화나 테이블 연결 단서가 보이지 않아.

따라서 **이 참조 추적은 여기서 중단**하자. 내가 단서가 부족한 상태에서 주변 코드를 반복해서 요청했어. 지금까지 보낸 내용은 충분하고, 같은 구간을 다시 보낼 필요 없어.

## 현재 확정된 상태

- `FUN_71006c2580`에는 실제 경과 시간·고정 간격·외부 설정값을 선택하는 분기가 있어.
- 선택한 시간값을 다음 경로로 저장해.
  ```c
  참조객체 = *(long *)(현재객체 + 0x30);
  실제객체 = *(long *)(참조객체 + 0x28);
  *(float *)(실제객체 + 0x20) = 선택한_시간값;
```

- 하지만 현재 객체의 생성 위치와 `+0x30` 필드에 값을 넣는 곳은 아직 찾지 못했어.
- 오디오로 추정되는 하위 처리만으로 게임 전체 속도 보정 여부를 판단할 수는 없어.


## 다음 방향을 정하는 데 필요한 정보

지금 Ghidra로 파일만 보고 있는지, 아니면 실행 중인 게임에 디버거를 연결해서 메모리·레지스터도 확인할 수 있는지 알려줘.

```text
1. Ghidra로 정적 분석만 가능
2. 실행 중 메모리·레지스터 확인도 가능
```

가능한 환경에 맞춰 다음 작업을 하나만 정할게. 아직 패치 위치가 확정된 것은 아니야.

---

# Ghidra로 정적 분석만 가능

그냥 지금 까지 나온 단서로 임시 패치를 하나 만들수있냐

응. 지금까지 나온 코드만으로 **시험용 패치** 하나는 만들 수 있어. 가장 근거가 분명한 것은 `FUN_71006c2580`에서 고정 시간값 대신 실제 경과 시간을 선택하도록 하는 4바이트 패치야.

단, 이것은 FPS 제한 해제 패치도, 게임 전체 속도 보정을 보장하는 패치도 아니야. “이 함수가 공급하는 시간값을 실제 경과 시간으로 바꾸는 실험”으로 구분해야 해.

## 제안하는 패치 하나

| 항목 | 내용 |
| :-- | :-- |
| Ghidra 주소 | `71006c2b4c` |
| 원본 명령 | `cbz x8, LAB_71006c2b58` |
| 변경 명령 | `b 0x71006c2b60` |
| 원본 바이트 | `68 00 00 B4` |
| 변경 바이트 | `05 00 00 14` |

원본 4바이트가 일치하는지 먼저 확인해. 다르면 적용하지 마.

변경 후 흐름은 이렇게 돼.

```asm
71006c2b4c  b     0x71006c2b60

; 외부 설정값과 고정 간격 선택을 건너뛰고 여기로 이동
71006c2b60  sub   x8,x0,x28
71006c2b64  ucvtf d1,x8
71006c2b68  fdiv  d0,d1,d0
71006c2b6c  fcvt  s0,d0
71006c2b70  b     LAB_71006c2b84

71006c2b84  str   s0,[x10,#0x18]
```

보내준 코드에서 이 경로는 다음 값을 계산해.

```c
시간값 = (현재틱 - 이전틱) / 틱주파수;
```

이 위치에서는 나눗셈의 분모인 `d0`에 이미 틱주파수가 준비돼 있어. 따라서 기존 계산 경로를 재사용하며, 새 계산 코드를 삽입하지 않아도 돼.

## 무엇을 바꾸고, 무엇을 유지하나

바뀌는 것:

- 외부 설정 구조체의 `+0x3c` 시간값을 사용하지 않음.
- `1 / 목표 주파수`인 고정 간격도 사용하지 않음.
- 대신 대기 처리가 끝난 시점까지의 실제 경과 시간을 사용.

유지되는 것:

- 앞쪽의 프레임 제한·대기 루프.
- 전역 설정값 `1`·`2`와 그 복사 경로.
- 뒤쪽의 조건부 시간값 0 처리.
- 시간값 저장과 하위 함수 전달.

그래서 이 패치 하나로 30FPS 제한이 60FPS로 풀리지는 않아. 목표 주파수 설정은 그대로야.

## Ghidra에서 입력하는 방법

원본을 보존한 복사본에서 작업해.

1. Listing에서 `G` → `71006c2b4c`.
2. 원본이 아래와 같은지 확인.

```asm
cbz x8,LAB_71006c2b58
```

3. 해당 명령 우클릭 → `Patch Instruction`.
4. 다음 명령 입력.

```asm
b 0x71006c2b60
```

5. 결과가 아래처럼 나오는지 확인.

```text
05 00 00 14    b LAB_71006c2b60
```


Ghidra에서는 Listing의 `Patch Instruction`으로 명령을 수정할 수 있어. 다만 프로젝트 안의 수정과 실제 실행 파일로 내보내는 작업은 별개야.[^15_1][^15_2]

## 시험할 때 주의할 점

현재 확보한 실제 경과 시간 경로에는 큰 시간값을 제한하는 처리가 보이지 않아. 따라서 로딩·일시정지·긴 프레임 지연 뒤에는 큰 시간값이 전달될 수 있어. 오디오 쪽으로 추정한 타이머가 영향을 받는 것도 이미 확인했어.

시험할 때는 다른 패치와 섞지 말고, 다음을 비교하는 게 좋아.

- 평상시 게임 진행 속도가 달라지는지.
- FPS가 떨어질 때 느려지는 현상이 줄어드는지.
- 메뉴·로딩 복귀 후 갑작스러운 진행이나 이상이 생기는지.
- 소리·환경음 타이밍만 달라지고 이동 속도는 그대로인지.

문제가 생기면 아래 원본 4바이트로 되돌리면 돼.

```text
71006c2b4c: 68 00 00 B4
```

지금 제시한 주소는 Ghidra 가상주소야. 실행용 치트 코드나 파일 오프셋으로 그대로 사용하면 안 돼. 그 형식으로 만들려면 적용 방식과 모듈 기준 주소를 별도로 확인해야 해.

<span style="display:none">[^15_10][^15_11][^15_12][^15_13][^15_14][^15_15][^15_3][^15_4][^15_5][^15_6][^15_7][^15_8][^15_9]</span>

<div align="center">⁂</div>

[^15_1]: https://www.tripwire.com/state-of-security/ghidra-101-binary-patching

[^15_2]: https://jorianwoltjer.com/blog/p/stories/introduction-to-reverse-engineering-with-ghidra

[^15_3]: https://llvm.org/devmtg/2012-11/Northover-AArch64.pdf

[^15_4]: https://www.scs.stanford.edu/~zyedidia/arm64/br.html

[^15_5]: https://coalfire.com/the-coalfire-blog/reverse-engineering-and-patching-with-ghidra

[^15_6]: https://materials.rangeforce.com/tutorial/2020/04/12/Patching-Binaries/

[^15_7]: https://blog.cjearls.io/2019/04/editing-executable-binary-file-with.html

[^15_8]: https://github.com/NationalSecurityAgency/ghidra/discussions/3691

[^15_9]: https://boricj.net/reverse-engineering/2023/07/03/part-6.html

[^15_10]: https://github.com/MrBadasss/re-course/blob/main/GHIDRA.md

[^15_11]: https://www.reverseengineering.app/en/blog/ghidra-tutorial-for-beginners

[^15_12]: https://www.reddit.com/r/ghidra/comments/1oubdz8/is_there_any_way_to_directly_modify_operation/

[^15_13]: https://blog.dhilipsanjay.in/ctfs/tryhackme/tryhackme/ccghidra

[^15_14]: https://github.com/Ilias1988/Hacking-Cheatsheets/blob/main/Ghidra/README.md

[^15_15]: https://github.com/zernanvash/cheatsheet/blob/main/reverse-engineering-vault/tools/Ghidra Cheat Sheet.md


---

# 지금 내가 원하는건 30FPS면 30FPS로 시간이 연동되는거야

아, 네가 원하는 건 **실제 FPS에 맞춰 게임 진행 시간도 연동되는 것**이구나. 60FPS로 강제하는 게 아니라, 30FPS로 실행돼도 게임 속도는 정상으로 유지하려는 거지.

예를 들어 다음처럼 말이야.


| 실제 FPS | 한 번의 갱신에서 진행해야 하는 시간 |
| --: | --: |
| 60FPS | 약 0.01667초 |
| 30FPS | 약 0.03333초 |
| 20FPS | 약 0.05초 |

30FPS인데 매번 `1/60초`만 진행하면 실제 1초 동안 게임은 0.5초만 진행하니까, 그 느려짐을 없애는 게 목표인 거야.

## 지금까지 찾은 코드와의 관계

보내준 코드에는 이미 설정값에 따라 아래 두 조합을 만드는 경로가 있어.

```text
설정값 1 → 목표 60 / 시간 간격 1/60초
설정값 2 → 목표 30 / 시간 간격 2/60초
```

다만 이것은 설정값에 따른 연동이야. 실제 성능 때문에 60FPS 목표에서 30FPS로 떨어졌을 때도 시간값이 자동으로 `1/30초`가 되는지는 확인되지 않았어.

## 방금 제안한 패치의 정확한 의미

방금 패치는 “실제 경과 시간을 선택한다”는 점에서 네 목표와 방향은 맞아.

```c
진행 시간 = 실제로 지난 시간;
```

하지만 우리가 확인한 하위 사용처가 주로 오디오 쪽으로 추정되기 때문에, 그 패치가 캐릭터 이동·애니메이션·물리까지 정상 속도로 맞춰 준다고 보장할 수는 없어. 내가 그 구분을 더 명확하게 했어야 했어.

따라서 현재 제안한 것은 게임 전체 FPS 연동 패치가 아니라, 시간 공급 경로 하나를 실제 시간으로 바꾸는 시험용 패치야. 네 목표는 이제 “FPS 제한 해제”가 아니라 “실제 FPS가 낮아져도 게임 진행 속도를 유지하는 시간 보정”으로 고정하고 이어갈게.

---

# 이제 그게 아니라 DYNAMIC FPS 패치 원한다했잖아 그리고 다른 참고사항으로 다른 게임의 DYNAMIC FPS 패치를 보여줫고

맞아. 네가 원한 건 처음부터 **DYNAMIC FPS 패치**였는데, 내가 중간에 목표를 놓치고 고정 30/60 설정이나 일부 시간값 변경 쪽으로 잘못 좁혔어. 같은 의도를 다시 설명하게 해서 미안해.

그리고 다른 게임의 DYNAMIC FPS 패치를 참고로 보여줬다는 점도 이해했어. 다만 지금 내가 볼 수 있는 대화 내용에는 그 참고 패치의 코드가 없어. 그 구현을 확인한 것처럼 말하거나 기억해서 재구성하지는 않을게.

## 지금까지의 결과는 이렇게 정정할게

- 고정 설정값 `1`·`2`와, 그 값으로 목표 주파수·시간 간격을 만드는 경로는 찾았어.
- 실제 경과 시간을 계산하는 경로도 찾았어.
- 하지만 이를 게임 전체의 갱신 시간에 연결하는 DYNAMIC FPS 패치는 아직 완성하지 못했어.
- 내가 제시한 `71006c2b4c → 71006c2b60` 변경은 검증되지 않은 부분 실험이야. 네가 요청한 DYNAMIC FPS 패치라고 취급하면 안 돼.


## 참고 패치를 기준으로 다시 맞추자

다른 게임의 참고 패치 코드와 함께 보냈던 설명을 한 번만 텍스트로 다시 붙여줘. 길면 핵심 패치 코드부터 보내도 돼.

그걸 기준으로 참고 패치가 실제로 무엇을 바꾸는지와, 지금 찾은 코드 중 어느 부분이 대응하는지를 먼저 비교할게. 더 이상 목적이 불분명한 주변 함수를 차례로 열게 하지는 않을게.

