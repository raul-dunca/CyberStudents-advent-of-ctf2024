<img src="https://github.com/raul-dunca/assets/blob/main/.images_CyberStudents-advent-of-ctf2024/day11_description.png">


This challenge was straightforward, I can associate each emoji with 1 letter from the 2023 encoded and deduce the rest of the emojis based on the words created. I developed a python script to decode the 2024 message:

```python
emoji_key = {
    "🦌": "D", "🔔": "E", "🎅": "A", "🎳": "R",
    "🎁": "C", "❄": "H", "⛄": "I", "🎶": "L",
    "🎤": "Y", "🎮": "O", "🎨": "U", "🎭": "T",
    "🕯️": "F", "🎵": "M", "🎰": "S", "🎲": "P",
    "🎸": "N", "🎬": "V", "🎥": "W", "🌟": "J", "🎊":"K","🎄":"B" ,"🎉":"G" ,"🎦":"X"
}

encoded_text= """
🦌🔔🎅🎳 🎁❄️⛄🎶🦌,

⛄ 🎰🔔🔔 🎤🎮🎨 🦌⛄🦌🎸'🎭 🦌🎮 🎭❄️🔔 🦌🎅⛄🎶🎤 🎁🎭🕯️, 🎊⛄🦌.

🎥🔔🎶🎶, 🎤🎮🎨 🎊🎸🎮🎥 🎥❄️🎅🎭 ❄️🎅🎲🎲🔔🎸🎰 🎸🎮🎥.

🎤🎮🎨 🎰🔔🔔 🎭❄️🎅🎭 🎲🔔🎳🎰🎮🎸 🎮🎨🎭🎰⛄🦌🔔 🎮🕯️ 🎤🎮🎨🎳 🎥⛄🎸🦌🎮🎥?

⛄🎸 🎭❄️🔔 🎥❄️⛄🎭🔔 🎬🎅🎸 🎥⛄🎭❄️ 🎭❄️🔔 🕯️🎳🔔🔔 🎁🎅🎸🦌🎤 🎰⛄🎉🎸?

⛄ ❄️🎅🦌 🎰🎮🎵🔔 🎶🔔🕯️🎭🎮🎬🔔🎳🎰 🕯️🎳🎮🎵 🎁❄️🎳⛄🎰🎭🎵🎅🎰 🎶🎅🎰🎭 🎤🔔🎅🎳.

🎭❄️🔔🎳🔔 🎥🔔🎳🔔 🎭🎮🎸🎰 🎮🕯️ 🎁❄️⛄🎶🦌🎳🔔🎸 🎥❄️🎮 🎲⛄🎁🎊🔔🦌 🎭❄️🔔 🎮🎭❄️🔔🎳 🎮🎲🎭⛄🎮🎸...

🎄🎨🎭 🎄🔔🕯️🎮🎳🔔 🎤🎮🎨 🎉🔔🎭 ⛄🎸, ⛄ 🎵🎨🎰🎭 🎅🎰🎊:

🎅 🕯️🎶🎅🎉 🎮🎳 🎤🎮🎨🎳 🕯️🎅🎵⛄🎶🎤?

🎄🎨🎭 🎅🎳🔔 🎤🎮🎨 🎰🎨-- 🎮❄️. 🎤🎮🎨 🎥🎅🎸🎭 🎭❄️🔔 🕯️🎶🎅🎉? 🎉🎮🎮🦌 🎁❄️🎮⛄🎁🔔. 🎁🎰🦌{🎄🎅🎉_🎄⛄🎉_🌟🎅🎥_🎄🎮🎦_🎥🔔🎄_🎬🎮🎥_🎥🎅🎦_🎄🎅🎉🎉🎤_🎥🎅🎬🎤_🎥🎮🎬🔔🎸_🎉🎶🎮🎥}

⛄'🎶🎶 🎅🎰🎊 🎵🎤 🔔🎶🎬🔔🎰 🎭🎮 🎭🎅🎊🔔 🎁🎅🎳🔔 🎮🕯️ 🎤🎮🎨🎳 🕯️🎅🎵⛄🎶🎤 🎸🎮🎥.

🦌🎮🎸'🎭 🕯️🎮🎳🎉🔔🎭 🎭🎮 ❄️🎅🎬🔔 🎰🎮🎵🔔 🕯️🎅🎵⛄🎶🎤 🎭⛄🎵🔔!

❄️🎅🎬🔔 🎅 ❄️🎮🎶🎶🎤 🌟🎮🎶🎶🎤 🎁❄️🎳⛄🎰🎭🎵🎅🎰!

🎰🎅🎸🎭🎅
"""


def decode_text(encoded_text, key):
    decoded = ""
    for char in encoded_text:
        if char in key:
            decoded += key[char]
        else:
            decoded += char  # Preserve characters not in the key (like punctuation)
    return decoded

# Decoding the 2024 ENCODED text
decoded_2024 = decode_text(encoded_text, emoji_key)
print(decoded_2024)
```


`CSD{BAG_BIG_JAW_BOX_WEB_VOW_WAX_BAGGY_WAVY_WOVEN_GLOW}`
