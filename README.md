                    section('၂။ ရွှေအလေးချိန်', [
                      Row(children: [
                        Expanded(child: field('ကျပ်သား', kyat)),
                        const SizedBox(width: 8),
                        Expanded(child: field('ပဲ', pae)),
                        const SizedBox(width: 8),
                        Expanded(child: field('ရွေး', yway)),
                      ]),
                      const Text('၁ ကျပ်သား = ၁၆ ပဲ = ၁၂၈ ရွေး'),
                    ]),
                    section('၃။ အလျော့တွက်', [
                      DropdownButtonFormField<String>(
                        value: wasteMode,
                        decoration: const InputDecoration(
                          labelText: 'အလျော့တွက်နည်း',
                          border: OutlineInputBorder()),
                        items: const [
                          DropdownMenuItem(value: 'weight',
                            child: Text('အလေးချိန်ဖြင့်')),
                          DropdownMenuItem(value: 'percent',
                            child: Text('ရာခိုင်နှုန်းဖြင့်')),
                          DropdownMenuItem(value: 'money',
                            child: Text('ငွေပမာဏဖြင့်')),
                        ],
                        onChanged: (v) => setState(() {
                          wasteMode = v!;
                        }),
                      ),
                      if (wasteMode == 'weight')
                        Row(children: [
                          Expanded(child: field('ကျပ်သား', wk)),
                          const SizedBox(width: 8),
                          Expanded(child: field('ပဲ', wp)),
                          const SizedBox(width: 8),
                          Expanded(child: field('ရွေး', wy)),
                        ])
                      else
                        field(wasteMode == 'percent'
                          ? 'အလျော့တွက် ရာခိုင်နှုန်း (%)'
                          : 'အလျော့တွက်ငွေ (ကျပ်)', wk),
                    ]),
                    section('၄။ လက်ခနှင့် အခြားငွေ', [
                      DropdownButtonFormField<String>(
                        value: laborMode,
                        decoration: const InputDecoration(
                          labelText: 'လက်ခတွက်နည်း',
                          border: OutlineInputBorder()),
                        items: const [
                          DropdownMenuItem(value: 'fixed',
                            child: Text('စုစုပေါင်း လက်ခ')),
                          DropdownMenuItem(value: 'perkyat',
                            child: Text('တစ်ကျပ်သားနှုန်း')),
                        ],
                        onChanged: (v) => setState(() {
                          laborMode = v!;
                        }),
                      ),
                      field('လက်ခ (ကျပ်)', labor),
                      field('အခြားကုန်ကျစရိတ် (ကျပ်)', extra),
                      field('လျှော့ပေးငွေ (ကျပ်)', discount),
                      field('အရေအတွက် (ခု)', qty),
                    ]),
                    Card(
                      color: const Color(0xFFFFF1CF),
                      child: Padding(
                        padding: const EdgeInsets.all(18),
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            const Text('စုစုပေါင်း ကျသင့်ငွေ'),
                            const SizedBox(height: 6),
                            Text(money(total),
                              style: const TextStyle(
                                fontSize: 27,
                                fontWeight: FontWeight.bold)),
                            const Divider(),
                            result('မူလရွှေဖိုး', money(baseAll)),
                            result('အလျော့တွက်ဖိုး', money(wasteAll)),
                            result('လက်ခ', money(laborAll)),
                            result('အခြားကုန်ကျစရိတ်', money(extraAll)),
                            result('လျှော့ပေးငွေ', money(val(discount))),
                            result('တွက်ထားသော အလေးချိန်',
                              '${(w + added).toStringAsFixed(4)} ကျပ်သား'),
                          ],
                        ),
                      ),
                    ),
                    const Text(
                      'မှတ်ချက် - ဆိုင်၏ အလျော့တွက်နှင့် လက်ခတွက်နည်းကို '
                      'အတည်ပြုပါ။ ဤ App သည် ကိုယ်တိုင်ထည့်သော '
                      'တန်ဖိုးများကို အခြေခံတွက်ချက်ပေးသည်။',
                      style: TextStyle(fontSize: 12)),
                  ],
                ),
              );
            }

            Widget result(String label, String value) =>
              Padding(
                padding: const EdgeInsets.symmetric(vertical: 4),
                child: Row(
                  mainAxisAlignment: MainAxisAlignment.spaceBetween,
                  children: [
                    Expanded(child: Text(label)),
                    const SizedBox(width: 8),
                    Flexible(child: Text(value,
                      textAlign: TextAlign.end)),
                  ],
                ),
              );
          }
          DART

      - name: Build Android APK
        run: |
          cd app
          flutter pub get
          flutter build apk --release

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: shwe-calculator-apk
          path: app/build/app/outputs/flutter-apk/app-release.apk
          flutter-apk/app-release.apk
          
