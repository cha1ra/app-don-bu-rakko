<template>
  <v-container>
    <v-snackbar
      v-model="snackbar"
      color="success"
    >
      コピーされました！

      <template #action="{ attrs }">
        <v-btn
          dark
          text
          v-bind="attrs"
          @click="snackbar = false"
        >
          <v-icon>
            mdi-close
          </v-icon>
        </v-btn>
      </template>
    </v-snackbar>
    <v-row justify="center">
      <v-col
        cols="12"
        sm="8"
        md="6"
      >
        <v-card class="pa-4 mb-4">
          <div class="title font-weight-bold mb-4">
            全角↔半角 カタカナ英数字︎ 変換
          </div>
          <v-form
            ref="form"
          >
            <v-textarea
              v-model="beforeText"
              placeholder="変換したい文字列を入力してください"
              :counter="1000"
              outlined
              :rules="formRules"
              @input="convert(beforeText)"
            />
          </v-form>

          <div class="mb-6 d-flex justify-center">
            <v-icon x-large class="mr-4">
              mdi-arrow-down
            </v-icon>
            <v-radio-group
              v-model="toType"
              row
              @change="convert(beforeText)"
            >
              <v-radio
                value="hankaku"
                label="半角カタカナに変換"
              />
              <v-radio
                value="zenkaku"
                label="全角カタカナに変換"
              />
            </v-radio-group>
          </div>
          <v-textarea
            v-model="afterText"
            placeholder="変換結果が表示されます"
            outlined
            hide-details
            readonly
            class="mb-2"
          />
          <div class="text-right">
            <v-btn
              elevation="1"
              class="font-weight-bold"
              color="primary"
              @click="copy"
            >
              <v-icon left>
                mdi-clipboard
              </v-icon>
              結果をコピーする
            </v-btn>
          </div>
        </v-card>
        <v-card class="pa-4">
          <div class="font-weight-bold">
            このツールについて
          </div>
          <div class="body-2 mb-6 grey--text text--darken-2">
            全角カタカナ&全角英数字 と 半角カタカナ&半角英数字 を変換するツールです。<br>
            対応している記号は以下の通りです。
          </div>
          <section class="mb-4">
            <div class="font-weight-bold">
              全角文字
            </div>
            <v-divider />
            <div class="caption">
              アイウエオカキクケコサシスセソタチツテトナニヌネノハヒフヘホマミムメモヤユヨラリルレロワヲンァィゥェォッャュョー。、「」ガギグゲゴザジズゼゾダヂヅデドバビブベボパピプペポヴヷヺ０１２３４５６７８９ＡＢＣＤＥＦＧＨＩＪＫＬＭＮＯＰＱＲＳＴＵＶＷＸＹＺａｂｃｄｅｆｇｈｉｊｋｌｍｎｏｐｑｒｓｔｕｖｗｘｙｚ
            </div>
          </section>
          <section class="mb-4">
            <div class="font-weight-bold">
              半角文字
            </div>
            <v-divider />
            <div class="caption">
              ｱｲｳｴｵｶｷｸｹｺｻｼｽｾｿﾀﾁﾂﾃﾄﾅﾆﾇﾈﾉﾊﾋﾌﾍﾎﾏﾐﾑﾒﾓﾔﾕﾖﾗﾘﾙﾚﾛﾜｦﾝｧｨｩｪｫｯｬｭｮｰ｡､｢｣ｶﾞｷﾞｸﾞｹﾞｺﾞｻﾞｼﾞｽﾞｾﾞｿﾞﾀﾞﾁﾞﾂﾞﾃﾞﾄﾞﾊﾞﾋﾞﾌﾞﾍﾞﾎﾞﾊﾟﾋﾟﾌﾟﾍﾟﾎﾟｳﾞﾜﾞｦﾞ0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz
            </div>
          </section>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import {
  defineComponent,
  ref,
  computed
} from '@nuxtjs/composition-api'

export default defineComponent({

  setup () {
    const beforeText = ref('')
    const afterText = ref('')

    const toType = ref('hankaku') // hankaku or zenkaku
    const fromType = computed(() => toType.value === 'hankaku' ? 'zenkaku' : 'hankaku')

    const charDict = {
      zenkaku: [
        'ア', 'イ', 'ウ', 'エ', 'オ', 'カ', 'キ', 'ク', 'ケ', 'コ', 'サ', 'シ', 'ス', 'セ', 'ソ',
        'タ', 'チ', 'ツ', 'テ', 'ト', 'ナ', 'ニ', 'ヌ', 'ネ', 'ノ', 'ハ', 'ヒ', 'フ', 'ヘ', 'ホ',
        'マ', 'ミ', 'ム', 'メ', 'モ', 'ヤ', 'ユ', 'ヨ', 'ラ', 'リ', 'ル', 'レ', 'ロ', 'ワ', 'ヲ', 'ン',
        'ァ', 'ィ', 'ゥ', 'ェ', 'ォ', 'ッ', 'ャ', 'ュ', 'ョ', 'ー', '。', '、', '「', '」',
        'ガ', 'ギ', 'グ', 'ゲ', 'ゴ', 'ザ', 'ジ', 'ズ', 'ゼ', 'ゾ', 'ダ', 'ヂ', 'ヅ', 'デ', 'ド',
        'バ', 'ビ', 'ブ', 'ベ', 'ボ', 'パ', 'ピ', 'プ', 'ペ', 'ポ', 'ヴ', 'ヷ', 'ヺ',
        '０', '１', '２', '３', '４', '５', '６', '７', '８', '９',
        'Ａ', 'Ｂ', 'Ｃ', 'Ｄ', 'Ｅ', 'Ｆ', 'Ｇ', 'Ｈ', 'Ｉ', 'Ｊ', 'Ｋ', 'Ｌ', 'Ｍ', 'Ｎ',
        'Ｏ', 'Ｐ', 'Ｑ', 'Ｒ', 'Ｓ', 'Ｔ', 'Ｕ', 'Ｖ', 'Ｗ', 'Ｘ', 'Ｙ', 'Ｚ',
        'ａ', 'ｂ', 'ｃ', 'ｄ', 'ｅ', 'ｆ', 'ｇ', 'ｈ', 'ｉ', 'ｊ', 'ｋ', 'ｌ', 'ｍ', 'ｎ',
        'ｏ', 'ｐ', 'ｑ', 'ｒ', 'ｓ', 'ｔ', 'ｕ', 'ｖ', 'ｗ', 'ｘ', 'ｙ', 'ｚ'
      ],
      hankaku: [
        'ｱ', 'ｲ', 'ｳ', 'ｴ', 'ｵ', 'ｶ', 'ｷ', 'ｸ', 'ｹ', 'ｺ', 'ｻ', 'ｼ', 'ｽ', 'ｾ', 'ｿ',
        'ﾀ', 'ﾁ', 'ﾂ', 'ﾃ', 'ﾄ', 'ﾅ', 'ﾆ', 'ﾇ', 'ﾈ', 'ﾉ', 'ﾊ', 'ﾋ', 'ﾌ', 'ﾍ', 'ﾎ',
        'ﾏ', 'ﾐ', 'ﾑ', 'ﾒ', 'ﾓ', 'ﾔ', 'ﾕ', 'ﾖ', 'ﾗ', 'ﾘ', 'ﾙ', 'ﾚ', 'ﾛ', 'ﾜ', 'ｦ', 'ﾝ',
        'ｧ', 'ｨ', 'ｩ', 'ｪ', 'ｫ', 'ｯ', 'ｬ', 'ｭ', 'ｮ', 'ｰ', '｡', '､', '｢', '｣',
        'ｶﾞ', 'ｷﾞ', 'ｸﾞ', 'ｹﾞ', 'ｺﾞ', 'ｻﾞ', 'ｼﾞ', 'ｽﾞ', 'ｾﾞ', 'ｿﾞ', 'ﾀﾞ', 'ﾁﾞ', 'ﾂﾞ', 'ﾃﾞ', 'ﾄﾞ',
        'ﾊﾞ', 'ﾋﾞ', 'ﾌﾞ', 'ﾍﾞ', 'ﾎﾞ', 'ﾊﾟ', 'ﾋﾟ', 'ﾌﾟ', 'ﾍﾟ', 'ﾎﾟ', 'ｳﾞ', 'ﾜﾞ', 'ｦﾞ',
        '0', '1', '2', '3', '4', '5', '6', '7', '8', '9',
        'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N',
        'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z',
        'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n',
        'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z'
      ]
    }

    const regExp = computed(() => new RegExp(`(${charDict[fromType.value].join('|')})`, 'g'))

    const convert = (text:string) => {
      if (!form.value.validate()) { return false }
      const convertedText = text.replace(regExp.value, (match) => {
        const index = charDict[fromType.value].indexOf(match)
        return charDict[toType.value][index]
      })
      afterText.value = convertedText
    }

    const form = ref(undefined)
    const formRules = [
      v => !!v || '値の入力が必要です',
      v => (v && v.length <= 1000) || '入力可能な文字列は1000文字までです'
    ]

    const snackbar = ref(false)
    const copy = () => {
      navigator.clipboard.writeText(afterText.value)
      snackbar.value = true
    }

    return {
      beforeText,
      afterText,
      toType,
      convert,
      form,
      formRules,
      snackbar,
      copy
    }
  },
  head: {
    title: '全角半角変換 - カタカナ、英数字 変換ツール',
    meta: [
      { hid: 'description', name: 'description', content: '全角カタカナ&全角英数字 と 半角カタカナ&半角英数字 を変換するツールです。' }
    ]
  }
})
</script>

<style lang="scss">

</style>
