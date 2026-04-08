import React, { useEffect, useMemo, useState } from "react";

const LEGACY_COLL = [
  { n: "Bleu de Dua Attar", h: "The Dua Brand", d: ["blue", "fresh", "aquatic", "woody"], o: ["Office", "Casual", "Outdoors"], s: "All Season", r: 5, i: "Bleu de Chanel" },
  { n: "Khamrah Qahwa", h: "Lattafa", d: ["gourmand", "coffee", "amber", "oriental"], o: ["Evening", "Date"], s: "Fall/Winter", r: 5, i: "" },
  { n: "Ottoman Breeze", h: "The Dua Brand", d: ["fresh", "aquatic", "blue", "green"], o: ["Casual", "Outdoors", "Office"], s: "Spring/Summer", r: 5, i: "Nishane Hacivat" },
  { n: "His Perspective Extrait", h: "The Dua Brand", d: ["woody", "fresh", "blue", "amber"], o: ["Office", "Casual", "Date"], s: "All Season", r: 5, i: "Prada Paradigme" },
  { n: "Jean Lowe Immortal", h: "Maison Alhambra", d: ["woody", "amber", "sweet", "vanilla"], o: ["Office", "Casual", "Date"], s: "Spring/Summer", r: 5, i: "LV L'Immensité" },
  { n: "MIRIS No54602", h: "MIRIS Oils", d: ["fresh", "green", "woody", "citrus"], o: ["Casual", "Office", "Outdoors"], s: "Spring/Summer", r: 5, i: "Nishane Hacivat" },
  { n: "Ameer Al Oudh Intense", h: "Lattafa", d: ["oud", "amber", "oriental", "smoky"], o: ["Evening", "Special Event"], s: "Fall/Winter", r: 4, i: "MFK By the Fireplace" },
  { n: "River Fougere", h: "The Dua Brand", d: ["fougere", "fresh", "green", "lavender"], o: ["Office", "Casual", "Outdoors"], s: "Spring/Fall", r: 4, i: "YSL Rive Gauche" },
  { n: "Gentleman's Club", h: "The Dua Brand", d: ["fougere", "tobacco", "woody", "classic"], o: ["Office", "Evening", "Date"], s: "Fall", r: 4, i: "Givenchy Gentleman Society" },
  { n: "Linen Vetiver", h: "Banana Republic", d: ["vetiver", "linen", "clean", "fresh"], o: ["Office", "Casual"], s: "Spring/Summer", r: 4, i: "" },
  { n: "Grassland", h: "Banana Republic", d: ["green", "fresh", "grassy", "woody"], o: ["Casual", "Outdoors"], s: "Spring/Summer", r: 4, i: "Creed Green Irish Tweed" },
  { n: "Vintage Green 78", h: "Banana Republic", d: ["green", "vintage", "woody", "herbal"], o: ["Casual", "Outdoors"], s: "Spring", r: 4, i: "" },
  { n: "Tobacco & Tonka Bean", h: "Banana Republic", d: ["tobacco", "tonka", "warm", "amber"], o: ["Casual", "Evening"], s: "Fall/Winter", r: 4, i: "" },
  { n: "Midnight Hour", h: "Banana Republic", d: ["dark", "woody", "amber", "mysterious"], o: ["Evening", "Date"], s: "Fall/Winter", r: 4, i: "" },
  { n: "Dark Cherry & Amber", h: "Banana Republic", d: ["cherry", "amber", "dark", "fruity"], o: ["Date", "Evening"], s: "Fall", r: 3, i: "" },
  { n: "Metal Rain", h: "Banana Republic", d: ["fresh", "metallic", "rain", "ozonic"], o: ["Casual", "Outdoors", "Office"], s: "Spring/Summer", r: 3, i: "" },
  { n: "MIRIS No56902", h: "MIRIS Oils", d: ["apple", "brandy", "woody", "gourmand"], o: ["Evening", "Date"], s: "Fall", r: 4, i: "Kilian Apple Brandy" },
  { n: "MIRIS No47319", h: "MIRIS Oils", d: ["lavender", "vanilla", "spicy", "sweet"], o: ["Date", "Evening", "Casual"], s: "Fall/Winter", r: 4, i: "JPG Ultra Male" },
  { n: "MIRIS No59797", h: "MIRIS Oils", d: ["fresh", "blue", "woody", "metallic"], o: ["Office", "Casual"], s: "Spring/Summer", r: 4, i: "CH Bad Boy Cobalt" },
  { n: "Poseidon's Swimming Cologne", h: "The Dua Brand", d: ["aquatic", "fresh", "green", "marine"], o: ["Casual", "Outdoors"], s: "Summer", r: 4, i: "Aventus Cologne" },
  { n: "Bleu de Dua Exclusive Extrait", h: "The Dua Brand", d: ["blue", "fresh", "aquatic", "woody"], o: ["Office", "Date", "Evening"], s: "Fall/Winter", r: 4, i: "Bleu de Chanel Exclusif" },
  { n: "Jazz", h: "The Dua Brand", d: ["tobacco", "woody", "vanilla", "dark"], o: ["Evening", "Date"], s: "Fall/Winter", r: 4, i: "MFK Jazz Club" },
  { n: "Midnight Rendezvous", h: "The Dua Brand", d: ["amber", "oriental", "sweet", "tobacco"], o: ["Evening", "Date"], s: "Fall/Winter", r: 4, i: "YSL La Nuit de L'Homme" },
  { n: "Aphrodisiac", h: "The Dua Brand", d: ["amber", "musk", "sweet", "woody"], o: ["Date", "Evening"], s: "Fall/Winter", r: 4, i: "Initio Psychedelic Love" },
  { n: "Casino Royale Nights Extrait", h: "The Dua Brand", d: ["amber", "tobacco", "woody", "elegant"], o: ["Evening", "Date", "Special Event"], s: "Fall/Winter", r: 4, i: "Baccarat Rouge 540 Extrait" },
  { n: "Bet On 67", h: "The Dua Brand", d: ["fresh", "sporty", "aquatic", "citrus"], o: ["Casual", "Outdoors"], s: "Spring/Summer", r: 4, i: "Ralph Lauren Polo 67" },
  { n: "Iconic Plums By The Fire", h: "The Dua Brand", d: ["plum", "smoky", "amber", "oriental"], o: ["Evening", "Date"], s: "Fall/Winter", r: 4, i: "Andy Warhol + By the Fireplace" },
  { n: "Rome In Green", h: "The Dua Brand", d: ["green", "fresh", "citrus", "herbal"], o: ["Casual", "Outdoors"], s: "Spring/Summer", r: 4, i: "Valentino Born in Roma Green" },
  { n: "Midnight Fig", h: "The Dua Brand", d: ["fig", "green", "fresh", "woody"], o: ["Evening", "Special Event"], s: "Fall/Winter", r: 4, i: "Nest Indigo" },
  { n: "Fortune", h: "The Dua Brand", d: ["woody", "amber", "oriental", "elegant"], o: ["Office", "Evening", "Date"], s: "Fall", r: 4, i: "Xerjoff Naxos" },
  { n: "Smoky Cuba Tabac", h: "The Dua Brand", d: ["tobacco", "smoky", "woody", "oriental"], o: ["Evening", "Date"], s: "Fall/Winter", r: 4, i: "Alghabra Cuban Tobacco" },
  { n: "1 & Only Jazz Club", h: "The Dua Brand", d: ["tobacco", "vanilla", "woody", "oriental"], o: ["Evening", "Date"], s: "Fall/Winter", r: 4, i: "The One + Jazz Club" },
  { n: "Drowning in Bleu de Dua Attar", h: "The Dua Brand", d: ["blue", "deep", "woody", "amber"], o: ["Date", "Evening", "Office"], s: "Spring/Fall", r: 4, i: "Bleu de Chanel + Nishane Ani" },
  { n: "Drowning Bleu de Savage Fierce Extrait", h: "The Dua Brand", d: ["blue", "fresh", "spicy", "intense"], o: ["Evening", "Special Event"], s: "Fall/Winter", r: 3, i: "Ani + Sauvage EDT + Fierce" },
  { n: "Acqua Di DUA", h: "The Dua Brand", d: ["aquatic", "fresh", "citrus", "woody"], o: ["Casual", "Outdoors", "Office"], s: "Spring/Summer", r: 4, i: "Acqua di Gio vintage" },
  { n: "Error 410", h: "The Dua Brand", d: ["gourmand", "amber", "sweet", "golden"], o: ["Date", "Evening", "Special Event"], s: "Fall/Winter", r: 4, i: "1 Million Absolutely Gold" },
  { n: "The Mobster vs Poseidon", h: "The Dua Brand", d: ["aquatic", "woody", "fresh", "elegant"], o: ["Date", "Evening", "Casual"], s: "Fall/Spring", r: 4, i: "Roja Creation-E x Creed Aventus" },
  { n: "Ottomans Vert Breeze", h: "The Dua Brand", d: ["fresh", "green", "woody", "aquatic"], o: ["Casual", "Office", "Outdoors"], s: "Spring/Summer", r: 4, i: "Hacivat x Green Irish Tweed" },
  { n: "Aroma of Heaven", h: "The Dua Brand", d: ["vetiver", "woody", "earthy", "fresh"], o: ["Office", "Casual", "Outdoors"], s: "All Season", r: 4, i: "Hermes Terre d'Hermes Vintage" },
  { n: "Poseidon's Ocean Mist", h: "The Dua Brand", d: ["aquatic", "fresh", "marine", "ozonic"], o: ["Casual", "Office", "Outdoors"], s: "Spring/Summer", r: 4, i: "Aventus Cologne x Nautica Voyage" },
  { n: "Imperiale Matrix", h: "The Dua Brand", d: ["fresh", "aquatic", "citrus", "elegant"], o: ["Office", "Casual", "Outdoors"], s: "Spring/Summer", r: 4, i: "Millesime Imperial x Xerjoff Nio" },
  { n: "The Savage Attar", h: "The Dua Brand", d: ["fresh", "spicy", "woody", "ambroxan"], o: ["Date", "Casual", "Office"], s: "All Season", r: 4, i: "Dior Sauvage Parfum" },
  { n: "Poseidon's Fireplace", h: "The Dua Brand", d: ["smoky", "woody", "aquatic", "amber"], o: ["Evening", "Casual"], s: "Fall", r: 3, i: "Aventus + By the Fireplace" },
  { n: "Bleu de Casino Elixir Extrait", h: "The Dua Brand", d: ["blue", "amber", "woody", "elegant"], o: ["Evening", "Office", "Date"], s: "Fall", r: 3, i: "Bleu de Chanel + Aventus + BR540" },
  { n: "Desert Reflection Extrait", h: "The Dua Brand", d: ["amber", "woody", "spicy", "oriental"], o: ["Evening", "Special Event"], s: "Fall/Winter", r: 3, i: "Amouage Reflection Man" },
  { n: "D Le Attar", h: "The Dua Brand", d: ["woody", "fresh", "intense", "elegant"], o: ["Office", "Date"], s: "Fall/Winter", r: 3, i: "YSL Y Le Parfum" },
  { n: "D Le Parfum Intense", h: "The Dua Brand", d: ["woody", "fresh", "intense", "formal"], o: ["Evening", "Date"], s: "Fall/Winter", r: 3, i: "YSL Y EDP Intense" },
  { n: "#Imagine", h: "The Dua Brand", d: ["fresh", "citrus", "aquatic", "airy"], o: ["Casual", "Office"], s: "Spring/Summer", r: 3, i: "LV Imagination" },
  { n: "Error 413", h: "The Dua Brand", d: ["gourmand", "amber", "sweet", "night"], o: ["Evening", "Casual"], s: "Fall/Winter", r: 3, i: "1 Million Lucky" },
  { n: "His Intense Aspiration", h: "The Dua Brand", d: ["woody", "fresh", "intense", "citrus"], o: ["Casual", "Office"], s: "Spring/Summer", r: 3, i: "Allure Homme Blanche" },
  { n: "His OG Aspiration", h: "The Dua Brand", d: ["fresh", "citrus", "woody", "classic"], o: ["Casual", "Office"], s: "Fall/Winter", r: 3, i: "Chanel Allure Homme vintage" },
  { n: "His Aspiration Extreme Sport", h: "The Dua Brand", d: ["fresh", "sporty", "citrus", "green"], o: ["Casual", "Outdoors"], s: "Spring/Summer", r: 3, i: "Allure Homme Sport Extreme" },
  { n: "His Royalty", h: "The Dua Brand", d: ["fresh", "light", "woody", "clean"], o: ["Office", "Casual"], s: "Spring/Summer", r: 3, i: "Prada L'Homme L'Eau" },
  { n: "His Loyalty", h: "The Dua Brand", d: ["blue", "fresh", "woody", "metallic"], o: ["Office", "Casual"], s: "Spring/Summer", r: 3, i: "D&G Devotion" },
  { n: "Fierce Savage", h: "The Dua Brand", d: ["fresh", "spicy", "ambroxan", "woody"], o: ["Casual", "Office", "Outdoors"], s: "All Season", r: 3, i: "Fierce + Dior Sauvage EDT" },
  { n: "Imperial Blue", h: "The Dua Brand", d: ["blue", "fresh", "citrus", "elegant"], o: ["Office", "Casual", "Date"], s: "Spring/Summer", r: 3, i: "Bleu de Chanel + Millesime Imperial" },
  { n: "Imperiale Casino Elixir", h: "The Dua Brand", d: ["amber", "aquatic", "elegant", "special"], o: ["Special Event", "Date"], s: "Spring/Fall", r: 3, i: "Millesime Imperial + Aventus + BR540" },
  { n: "Intuition Vision", h: "The Dua Brand", d: ["aquatic", "fresh", "clean", "woody"], o: ["Casual", "Office"], s: "Spring/Summer", r: 3, i: "Acqua di Gio Profumo" },
  { n: "Mystical Amulet of Blue", h: "The Dua Brand", d: ["blue", "fresh", "woody", "aquatic"], o: ["Casual", "Office"], s: "Spring/Summer", r: 3, i: "Ex Nihilo Blue Talisman" },
  { n: "Peace with Poseidon", h: "The Dua Brand", d: ["aquatic", "fresh", "marine", "ozonic"], o: ["Casual", "Outdoors"], s: "Spring/Summer", r: 3, i: "Scent of Peace + Aventus" },
  { n: "Victorian Poseidon", h: "The Dua Brand", d: ["aquatic", "fresh", "woody", "formal"], o: ["Special Event"], s: "Fall/Winter", r: 3, i: "Aventus + Xerjoff 1861 Renaissance" },
  { n: "Gone Swimming", h: "The Dua Brand", d: ["aquatic", "fresh", "green", "marine"], o: ["Casual", "Outdoors"], s: "Summer", r: 3, i: "LV Afternoon Swim" },
  { n: "Stronger With Azure Supernova 2.0", h: "The Dua Brand", d: ["blue", "fresh", "citrus", "ozonic"], o: ["Date", "Evening"], s: "Fall/Winter", r: 3, i: "Stronger With You + ADG Profondo" },
  { n: "Iconic Greenwich Village", h: "The Dua Brand", d: ["fresh", "green", "woody", "urban"], o: ["Casual", "Office"], s: "Spring/Fall", r: 3, i: "Bond No. 9 Greenwich Village" },
  { n: "Spice It Up Metallic Musk", h: "The Dua Brand", d: ["spicy", "musk", "metallic", "fresh"], o: ["Office", "Casual"], s: "Fall/Winter", r: 3, i: "Spicebomb Metallic Musk" },
  { n: "True Self Absolute", h: "The Dua Brand", d: ["woody", "amber", "clean", "evening"], o: ["Date", "Evening"], s: "Fall/Winter", r: 3, i: "YSL MYSLF L'Absolu" },
  { n: "Rome in Yellow Dream", h: "The Dua Brand", d: ["citrus", "fresh", "floral", "warm"], o: ["Casual", "Date"], s: "Spring/Summer", r: 3, i: "Valentino Born in Roma Yellow Dream" },
  { n: "Rome In Coral Fantasy", h: "The Dua Brand", d: ["fresh", "citrus", "warm", "casual"], o: ["Casual", "Outdoors"], s: "Spring/Summer", r: 3, i: "Valentino Born in Roma Coral Fantasy" },
  { n: "Poseidon's Cotton Candy", h: "The Dua Brand", d: ["gourmand", "sweet", "aquatic", "playful"], o: ["Casual"], s: "Spring/Summer", r: 3, i: "Aventus + Cotton Candy" },
  { n: "Dua's Water Primal Essence", h: "The Dua Brand", d: ["aquatic", "fresh", "citrus", "gym"], o: ["Casual", "Outdoors", "Office"], s: "Spring/Summer", r: 3, i: "Acqua di Gio Absolu" },
  { n: "Midnight Candy For 1 & Only", h: "The Dua Brand", d: ["gourmand", "sweet", "amber", "club"], o: ["Evening", "Date"], s: "Fall/Winter", r: 3, i: "La Nuit de L'Homme + Candies" },
  { n: "Cola Fizz Of Tonka Beans", h: "The Dua Brand", d: ["gourmand", "tonka", "sweet", "fizzy"], o: ["Casual"], s: "All Season", r: 3, i: "Mancera Tonka Cola" },
  { n: "Supernova Cologne Intense", h: "The Dua Brand", d: ["fresh", "aquatic", "citrus", "gym"], o: ["Casual", "Office", "Outdoors"], s: "Spring/Summer", r: 3, i: "Roja Elysium Eau Intense" },
  { n: "Wooden Lucky Charm", h: "The Dua Brand", d: ["woody", "amber", "clean", "evening"], o: ["Casual", "Evening"], s: "Fall/Winter", r: 3, i: "Dior Bois Talisman" },
  { n: "Queen Of The Chess", h: "The Dua Brand", d: ["chypre", "woody", "elegant", "dark"], o: ["Special Event", "Date", "Evening"], s: "Fall", r: 3, i: "Mind Games Queening" },
  { n: "Dua's SoHo", h: "The Dua Brand", d: ["fresh", "citrus", "urban", "woody"], o: ["Casual", "Office"], s: "Spring/Fall", r: 3, i: "Bond No. 9 Soho" },
  { n: "City of Dua", h: "The Dua Brand", d: ["woody", "amber", "elegant", "evening"], o: ["Casual", "Evening"], s: "Fall/Winter", r: 3, i: "LV City of Stars" },
  { n: "Black Widow", h: "The Dua Brand", d: ["oud", "dark", "amber", "oriental"], o: ["Evening", "Special Event"], s: "Fall/Winter", r: 3, i: "Kilian Black Phantom" },
  { n: "Black Origami", h: "Maison Alhambra", d: ["oud", "dark", "woody", "oriental"], o: ["Evening", "Special Event"], s: "Fall/Winter", r: 3, i: "Tom Ford Black Orchid" },
  { n: "Daring Blue For Life", h: "Maison Alhambra", d: ["blue", "fresh", "woody", "clean"], o: ["Casual", "Office"], s: "Spring/Summer", r: 3, i: "D&G Light Blue Forever" },
  { n: "Glacier Bold", h: "Maison Alhambra", d: ["fresh", "fruity", "aquatic", "bold"], o: ["Casual", "Outdoors"], s: "Spring/Summer", r: 3, i: "JPG Le Beau Le Parfum" },
  { n: "Glacier Le Noir", h: "Maison Alhambra", d: ["woody", "spicy", "dark", "sweet"], o: ["Date", "Evening"], s: "Fall/Winter", r: 3, i: "JPG Le Male Le Parfum" },
  { n: "Hercules", h: "Maison Alhambra", d: ["tobacco", "woody", "amber", "elegant"], o: ["Evening", "Date"], s: "Fall/Winter", r: 3, i: "Parfums de Marly Herod" },
  { n: "Jean Lowe Vibe", h: "Maison Alhambra", d: ["fresh", "citrus", "aquatic", "tropical"], o: ["Casual", "Outdoors"], s: "Spring/Summer", r: 3, i: "LV Pacific Chill" },
  { n: "Kismet Angel", h: "Maison Alhambra", d: ["sweet", "vanilla", "amber", "boozy"], o: ["Evening", "Date"], s: "Fall/Winter", r: 3, i: "Kilian Angel's Share" },
  { n: "Perseus", h: "Maison Alhambra", d: ["woody", "clean", "musk", "powdery"], o: ["Office", "Casual", "Date"], s: "All Season", r: 3, i: "Parfums de Marly Pegasus" },
  { n: "Salvo Elixir", h: "Maison Alhambra", d: ["spicy", "woody", "pepper", "ambroxan"], o: ["Date", "Evening", "Special Event"], s: "Fall/Winter", r: 4, i: "Dior Sauvage Elixir" },
  { n: "Salvo Intense", h: "Maison Alhambra", d: ["fresh", "spicy", "ambroxan", "lavender"], o: ["Casual", "Office"], s: "All Season", r: 3, i: "Dior Sauvage EDP" },
  { n: "Tobacco Touch", h: "Maison Alhambra", d: ["tobacco", "vanilla", "sweet", "warm"], o: ["Evening", "Date"], s: "Fall/Winter", r: 4, i: "Tom Ford Tobacco Vanille" },
  { n: "Victorioso", h: "Maison Alhambra", d: ["fresh", "aquatic", "woody", "sporty"], o: ["Casual", "Outdoors"], s: "Spring/Summer", r: 4, i: "Paco Rabanne Invictus" },
  { n: "Victorioso Myth", h: "Maison Alhambra", d: ["amber", "woody", "elegant", "dark"], o: ["Date", "Evening"], s: "Fall/Winter", r: 3, i: "Invictus Legend" },
  { n: "Victorioso Nero", h: "Maison Alhambra", d: ["amber", "tobacco", "dark", "woody"], o: ["Date", "Evening"], s: "Fall/Winter", r: 3, i: "Invictus Victory Elixir" },
  { n: "Yeah Man", h: "Maison Alhambra", d: ["fresh", "woody", "citrus", "clean"], o: ["Office", "Casual"], s: "All Season", r: 3, i: "YSL Y EDP" },
  { n: "Your Touch Amber", h: "Maison Alhambra", d: ["amber", "vanilla", "warm", "sweet"], o: ["Date", "Evening"], s: "Fall/Winter", r: 3, i: "Armani Stronger With You Amber" },
  { n: "Your Touch Oud", h: "Maison Alhambra", d: ["oud", "amber", "oriental", "dark"], o: ["Evening", "Special Event"], s: "Fall/Winter", r: 3, i: "Armani Stronger With You Oud" },
  { n: "Your Touch Sandal", h: "Maison Alhambra", d: ["sandalwood", "warm", "clean", "elegant"], o: ["Casual", "Date", "Evening"], s: "All Season", r: 3, i: "Armani Stronger With You Sandalwood" },
  { n: "Opulent Dubai God of Fire", h: "Lattafa", d: ["amber", "oud", "intense", "oriental"], o: ["Evening", "Special Event"], s: "Fall/Winter", r: 4, i: "" },
  { n: "Mashrabya", h: "Lattafa", d: ["amber", "smoky", "tobacco", "oriental"], o: ["Evening", "Date"], s: "Fall/Winter", r: 3, i: "Initio Smoking Hot" },
  { n: "Vintage Radio", h: "Lattafa", d: ["amber", "woody", "elegant", "date"], o: ["Date", "Evening"], s: "Fall/Winter", r: 4, i: "Initio Paragon" },
  { n: "Najdia Intense", h: "Lattafa", d: ["fresh", "fruity", "aquatic", "tropical"], o: ["Casual", "Outdoors"], s: "Spring/Summer", r: 3, i: "JPG Le Beau Paradise Garden" },
  { n: "Qaed Al Fursan", h: "Lattafa", d: ["fresh", "aquatic", "woody", "elegant"], o: ["Office", "Casual"], s: "Spring/Fall", r: 4, i: "Creed Aventus" },
  { n: "Hayatti Gold Elixir", h: "Lattafa", d: ["amber", "spicy", "woody", "club"], o: ["Date", "Evening"], s: "Fall/Winter", r: 3, i: "Armani Code Profumo" },
  { n: "Ana Abiyedh", h: "Lattafa", d: ["musk", "soft", "clean", "skin"], o: ["Casual", "Office"], s: "All Season", r: 3, i: "White Musk" },
  { n: "Teriaq Intense", h: "Lattafa", d: ["amber", "tobacco", "intense", "bold"], o: ["Evening", "Special Event"], s: "Fall/Winter", r: 4, i: "" },
  { n: "Liam Grey", h: "Lattafa", d: ["amber", "woody", "elegant", "chic"], o: ["Date", "Casual"], s: "Fall/Winter", r: 3, i: "BDK Gris Charnel" },
  { n: "Opulent Musk", h: "Lattafa", d: ["amber", "musk", "sweet", "evening"], o: ["Date", "Evening"], s: "All Season", r: 4, i: "BR540 x Initio Instant Crush" },
  { n: "Bade'e Al Oud For Glory", h: "Lattafa", d: ["oud", "saffron", "woody", "oriental"], o: ["Evening", "Special Event"], s: "Fall/Winter", r: 3, i: "Initio Oud for Greatness" },
  { n: "Bade'e Al Oud Sublime", h: "Lattafa", d: ["fresh", "fruity", "aquatic", "fun"], o: ["Casual", "Outdoors"], s: "Spring/Summer", r: 3, i: "Kayali Eden Juicy Apple" },
  { n: "Bade'e Al Oud Honor & Glory", h: "Lattafa", d: ["oud", "amber", "bold", "statement"], o: ["Evening", "Special Event"], s: "Fall/Winter", r: 3, i: "" },
  { n: "Bade'e Al Oud Amethyst", h: "Lattafa", d: ["oud", "floral", "rose", "romantic"], o: ["Date", "Evening"], s: "Spring/Fall", r: 3, i: "Initio Atomic Rose" },
  { n: "Bleu de Chanel EDT", h: "Designer", d: ["blue", "fresh", "woody", "citrus"], o: ["Office", "Casual", "Date"], s: "All Season", r: 4, i: "" },
  { n: "Montblanc Legend Spirit", h: "Designer", d: ["fresh", "aquatic", "clean", "light"], o: ["Casual", "Outdoors"], s: "Spring/Summer", r: 3, i: "" },
  { n: "Nautica Voyage", h: "Designer", d: ["aquatic", "fresh", "apple", "clean"], o: ["Casual", "Outdoors"], s: "Spring/Summer", r: 3, i: "" },
  { n: "Dior Sauvage EDP", h: "Designer", d: ["fresh", "spicy", "ambroxan", "lavender"], o: ["Office", "Casual", "Date"], s: "All Season", r: 4, i: "" },
  { n: "Davidoff Cool Water", h: "Designer", d: ["aquatic", "cool", "fresh", "classic"], o: ["Casual", "Outdoors"], s: "Spring/Summer", r: 3, i: "" },
  { n: "Valencia Uomo Intense", h: "Valencia / Millionaire", d: ["amber", "floral", "warm", "romantic"], o: ["Date", "Evening"], s: "Fall/Winter", r: 3, i: "Valentino Uomo Intense" },
  { n: "Valencia Uomo Fantasy", h: "Valencia / Millionaire", d: ["fresh", "citrus", "warm", "casual"], o: ["Casual", "Outdoors"], s: "Spring/Summer", r: 3, i: "Valentino Born in Roma Coral Fantasy" },
  { n: "Valencia Uomo", h: "Valencia / Millionaire", d: ["fresh", "woody", "casual", "clean"], o: ["Casual", "Office"], s: "Spring/Summer", r: 3, i: "Valentino Born in Roma" },
  { n: "Millionaire Royal", h: "Valencia / Millionaire", d: ["amber", "citrus", "gourmand", "club"], o: ["Date", "Evening"], s: "Fall/Winter", r: 3, i: "1 Million Royal" },
  { n: "Island Dreams", h: "Khadlaj", d: ["floral", "woody", "elegant", "upscale"], o: ["Casual", "Date", "Evening"], s: "Spring/Fall", r: 3, i: "LV Symphony" },
  { n: "Shiyaaka Snow", h: "Khadlaj", d: ["fresh", "aquatic", "clean", "office"], o: ["Office", "Casual"], s: "Spring/Summer", r: 3, i: "LV Meteore" },
  { n: "Shiyaaka Blue", h: "Khadlaj", d: ["blue", "fresh", "clean", "versatile"], o: ["Office", "Casual"], s: "All Season", r: 3, i: "Bleu de Chanel" },
  { n: "Epoque Artistique", h: "Khadlaj", d: ["amber", "tobacco", "dark", "formal"], o: ["Date", "Evening"], s: "Fall/Winter", r: 3, i: "Invictus Victory Elixir" },
  { n: "Haven Eclipse", h: "Mod Fragrances", d: ["woody", "amber", "elegant", "date"], o: ["Date", "Evening", "Office"], s: "Fall", r: 4, i: "Xerjoff Torino 22" },
  { n: "Secret Embers", h: "Mod Fragrances", d: ["amber", "spicy", "seductive", "evening"], o: ["Date", "Evening"], s: "Fall/Winter", r: 4, i: "Initio Side Effect" },
  { n: "Musk Melody", h: "Mod Fragrances", d: ["musk", "soft", "clean", "skin"], o: ["Casual", "Office"], s: "All Season", r: 3, i: "Initio Musk Therapy" },
  { n: "Reverence Extrait", h: "Mod Fragrances", d: ["woody", "clean", "musk", "elegant"], o: ["Office", "Casual", "Date"], s: "Spring/Fall", r: 4, i: "Parfums de Marly Percival" },
  { n: "The Exposed Extrait", h: "Mod Fragrances", d: ["bold", "evening", "statement", "dark"], o: ["Evening", "Special Event"], s: "Fall/Winter", r: 3, i: "Mind Games En Prise" },
  { n: "Fattan", h: "Rasasi", d: ["vetiver", "woody", "earthy", "herbal"], o: ["Office", "Casual"], s: "All Season", r: 3, i: "Terre d'Hermes" },
  { n: "Hawas Ice", h: "Rasasi", d: ["fresh", "icy", "aquatic", "sporty"], o: ["Casual", "Outdoors"], s: "Spring/Summer", r: 4, i: "" },
  { n: "Hawas Kobra", h: "Rasasi", d: ["aquatic", "fresh", "ozonic", "clean"], o: ["Casual", "Outdoors"], s: "Spring/Summer", r: 3, i: "LV Imagination" },
  { n: "Itqan Noir", h: "Zimaya", d: ["woody", "fresh", "dark", "elegant"], o: ["Date", "Evening"], s: "Fall/Winter", r: 3, i: "YSL MYSLF" },
  { n: "Reverie Aqua Pour Homme", h: "Zimaya", d: ["aquatic", "fresh", "casual", "clean"], o: ["Casual", "Office"], s: "Spring/Summer", r: 4, i: "Valentino Born in Roma Intense" },
  { n: "Abadi Opulent", h: "Zimaya", d: ["amber", "woody", "upscale", "evening"], o: ["Date", "Evening"], s: "Fall/Winter", r: 3, i: "YSL Y Elixir" },
  { n: "Wazir", h: "Yom & Layl", d: ["oud", "amber", "oriental", "upscale"], o: ["Date", "Evening"], s: "Fall/Winter", r: 4, i: "Mind Games Queening" },
  { n: "Raazi", h: "Yom & Layl", d: ["amber", "spicy", "social", "evening"], o: ["Evening", "Casual"], s: "Fall/Winter", r: 3, i: "Mind Games J'Adoube" },
  { n: "Rayees", h: "Yom & Layl", d: ["woody", "fresh", "office", "clean"], o: ["Office", "Casual"], s: "Spring/Fall", r: 3, i: "Mind Games Grand Masters" },
  { n: "Nor", h: "Yom & Layl", d: ["fresh", "clean", "light", "daytime"], o: ["Casual", "Office"], s: "Spring/Summer", r: 3, i: "Mind Games Lionora" },
  { n: "Limitless", h: "Yom & Layl", d: ["bold", "evening", "statement", "dark"], o: ["Evening", "Special Event"], s: "Fall/Winter", r: 3, i: "Mind Games Blockade" },
  { n: "Gladiator", h: "Yom & Layl", d: ["bold", "grand", "evening", "statement"], o: ["Special Event", "Evening"], s: "Fall/Winter", r: 3, i: "Argos Triumph of Bacchus" },
  { n: "Le Parfait", h: "Armaf", d: ["fresh", "green", "woody", "aquatic"], o: ["Office", "Casual"], s: "Spring/Summer", r: 4, i: "Creed Aventus x Green Irish Tweed" },
  { n: "Club de Nuit Milestone", h: "Armaf", d: ["fresh", "aquatic", "citrus", "elegant"], o: ["Casual", "Date"], s: "Spring/Summer", r: 4, i: "Creed Millesime Imperial" },
  { n: "Anti Social Parfum Club Most Wanted", h: "Singles", d: ["amber", "tobacco", "woody", "bold"], o: ["Date", "Evening"], s: "Fall/Winter", r: 4, i: "Azzaro The Most Wanted" },
  { n: "Afnan Supremacy Collector's Edition", h: "Singles", d: ["fresh", "aquatic", "fruity", "woody"], o: ["Casual", "Office", "Date"], s: "Spring/Fall", r: 4, i: "Creed Aventus Absolu" },
  { n: "Ajmal Evoke Gold", h: "Singles", d: ["woody", "fresh", "clean", "elegant"], o: ["Office", "Casual"], s: "All Season", r: 3, i: "Prada L'Homme" },
  { n: "Al Haramain Detour Noir", h: "Singles", d: ["amber", "spicy", "woody", "versatile"], o: ["Date", "Office"], s: "Fall/Winter", r: 3, i: "Parfums de Marly Layton" },
  { n: "Crown / Al-Rehab Silver", h: "Singles", d: ["fresh", "clean", "aquatic", "light"], o: ["Casual", "Office"], s: "Spring/Summer", r: 3, i: "Creed Silver Mountain Water" },
  { n: "Atralia Amazonas Avalanche", h: "Singles", d: ["woody", "floral", "intense", "formal"], o: ["Date", "Evening"], s: "Fall/Winter", r: 3, i: "Dior Homme Intense" },
  { n: "French Avenue Liquid Brun", h: "Singles", d: ["woody", "amber", "elegant", "refined"], o: ["Office", "Date", "Evening"], s: "Fall", r: 4, i: "Parfums de Marly Althair" },
  { n: "Game of Spades Wildcard", h: "Singles", d: ["amber", "urban", "evening", "bold"], o: ["Evening", "Special Event"], s: "Fall/Winter", r: 3, i: "Bond No. 9 Lafayette Street" },
  { n: "Grandeur Iconic Built", h: "Singles", d: ["electric", "blue", "bold", "club"], o: ["Evening", "Special Event"], s: "Fall/Winter", r: 3, i: "YSL Bleu Electrique" },
  { n: "Paris Corner Rifaqat", h: "Singles", d: ["musk", "soft", "cozy", "skin"], o: ["Casual", "Evening"], s: "Fall/Winter", r: 3, i: "YSL Babycat" },
  { n: "Rayhaan Aquatica", h: "Singles", d: ["aquatic", "citrus", "fresh", "beach"], o: ["Casual", "Outdoors"], s: "Spring/Summer", r: 3, i: "Creed Virgin Island Water" },
  { n: "Royalty by Maluma Onyx", h: "Singles", d: ["amber", "club", "evening", "bold"], o: ["Date", "Evening"], s: "Fall/Winter", r: 3, i: "1 Million Prive-adjacent" },
  { n: "Vurv Royce Black", h: "Singles", d: ["amber", "dark", "club", "bold"], o: ["Date", "Evening"], s: "Fall/Winter", r: 3, i: "1 Million Prive-adjacent" },
  { n: "Suits", h: "Fragrance World", d: ["fougere", "fresh", "lavender", "woody"], o: ["Office", "Casual"], s: "All Season", r: 4, i: "YSL Tuxedo" },
  { n: "Lust Cherry", h: "Fragrance World", d: ["cherry", "sweet", "dark", "romantic"], o: ["Date", "Evening"], s: "Fall/Winter", r: 3, i: "Tom Ford Lost Cherry" },
  { n: "Intense Peach", h: "Fragrance World", d: ["peach", "sweet", "bold", "evening"], o: ["Evening", "Date"], s: "Fall", r: 3, i: "Tom Ford Bitter Peach" },
  { n: "Oud Wonder", h: "Fragrance World", d: ["oud", "woody", "refined", "office"], o: ["Office", "Casual", "Date"], s: "Fall/Winter", r: 3, i: "Tom Ford Oud Wood" },
  { n: "Ebony Fume", h: "Fragrance World", d: ["smoky", "dark", "formal", "evening"], o: ["Evening", "Special Event"], s: "Fall/Winter", r: 4, i: "Tom Ford Ebene Fume" },
  { n: "Proud of You", h: "Fragrance World", d: ["woody", "fresh", "casual", "office"], o: ["Office", "Casual"], s: "Fall/Winter", r: 3, i: "Armani Stronger With You" },
  { n: "Proud of You Absolute", h: "Fragrance World", d: ["woody", "amber", "upscale", "evening"], o: ["Date", "Evening"], s: "Fall/Winter", r: 3, i: "Armani Stronger With You Absolutely" },
  { n: "La Uno Million Elixir", h: "Fragrance World", d: ["gourmand", "citrus", "amber", "club"], o: ["Date", "Evening"], s: "Fall/Winter", r: 4, i: "Paco Rabanne 1 Million Elixir" },
  { n: "Neroli Riviera", h: "Fragrance World", d: ["citrus", "neroli", "fresh", "beach"], o: ["Casual", "Outdoors"], s: "Spring/Summer", r: 4, i: "Tom Ford Neroli Portofino" },
  { n: "Prestige Of DUA Black", h: "The Dua Brand", d: ["woody", "spicy", "dark", "refined"], o: ["Evening", "Office"], s: "Fall/Winter", r: 3, i: "Pasha de Cartier Noire" },
  { n: "D Eau de Parfum Intense", h: "The Dua Brand", d: ["woody", "fresh", "aromatic", "intense"], o: ["Office", "Date"], s: "All Season", r: 3, i: "YSL Y Eau de Parfum Intense" },
  { n: "Intense Eau Homme", h: "The Dua Brand", d: ["woody", "amber", "clean", "masculine"], o: ["Office", "Date"], s: "All Season", r: 3, i: "Dior Homme Intense style" },
  { n: "Acqua Di Savage Elixir", h: "The Dua Brand", d: ["fresh", "spicy", "dark", "aquatic"], o: ["Date", "Evening"], s: "All Season", r: 3, i: "Acqua di Gio x Sauvage Elixir" },
  { n: "Reminiscent", h: "The Dua Brand", d: ["woody", "fresh", "clean", "elegant"], o: ["Office", "Casual"], s: "All Season", r: 3, i: "" },
  { n: "His Bleu Welsh Sea", h: "The Dua Brand", d: ["blue", "marine", "fresh", "woody"], o: ["Office", "Casual", "Outdoors"], s: "Spring/Summer", r: 3, i: "" },
  { n: "Ferocious Legend", h: "The Dua Brand", d: ["fresh", "woody", "fierce", "blue"], o: ["Casual", "Office"], s: "All Season", r: 3, i: "" },
  { n: "1 And Only Gentleman", h: "The Dua Brand", d: ["woody", "spicy", "gentleman", "amber"], o: ["Office", "Date", "Evening"], s: "Fall/Winter", r: 3, i: "" },
  { n: "Legend of the Blue", h: "The Dua Brand", d: ["blue", "fresh", "woody", "versatile"], o: ["Office", "Casual"], s: "Spring/Summer", r: 3, i: "" }
];

const PDF_COLLECTION = [
  { h: "The Dua Brand", n: "#Imagine" },
  { h: "The Dua Brand", n: "Acqua Di DUA" },
  { h: "The Dua Brand", n: "Bet On 67" },
  { h: "The Dua Brand", n: "Bleu de Casino Elixir Extrait" },
  { h: "The Dua Brand", n: "Bleu de Dua Attar" },
  { h: "The Dua Brand", n: "Bleu de Dua Exclusive Extrait" },
  { h: "The Dua Brand", n: "Casino Royale Nights Extrait" },
  { h: "The Dua Brand", n: "D Le Attar" },
  { h: "The Dua Brand", n: "D Le Parfum Intense" },
  { h: "The Dua Brand", n: "Desert Reflection Extrait" },
  { h: "The Dua Brand", n: "Drowning in Bleu de Dua Attar" },
  { h: "The Dua Brand", n: "Drowning in Bleu de Savage Fierce Extrait" },
  { h: "The Dua Brand", n: "His Intense Aspiration" },
  { h: "The Dua Brand", n: "His Loyalty" },
  { h: "The Dua Brand", n: "Fierce Savage" },
  { h: "The Dua Brand", n: "His Royalty" },
  { h: "The Dua Brand", n: "Error 410" },
  { h: "The Dua Brand", n: "Error 413" },
  { h: "The Dua Brand", n: "His Aspiration Extreme Sport" },
  { h: "The Dua Brand", n: "His OG Aspiration" },
  { h: "The Dua Brand", n: "His Perspective" },
  { h: "The Dua Brand", n: "Iconic Greenwich Village" },
  { h: "The Dua Brand", n: "Imperial Blue" },
  { h: "The Dua Brand", n: "Imperiale Casino Elixir" },
  { h: "The Dua Brand", n: "Intuition Vision" },
  { h: "The Dua Brand", n: "Jazz" },
  { h: "The Dua Brand", n: "Mystical Amulet of Blue" },
  { h: "The Dua Brand", n: "Peace with Poseidon" },
  { h: "The Dua Brand", n: "Poseidon's Swimming Cologne" },
  { h: "The Dua Brand", n: "Poseidon's Ocean Mist" },
  { h: "The Dua Brand", n: "River Fougere" },
  { h: "The Dua Brand", n: "Rome in Yellow Dream" },
  { h: "The Dua Brand", n: "Spice It Up: Metallic Musk" },
  { h: "The Dua Brand", n: "Stronger With Azure Supernova 2.0" },
  { h: "The Dua Brand", n: "The Savage Attar" },
  { h: "The Dua Brand", n: "Iconic Plums By The Fire" },
  { h: "The Dua Brand", n: "Poseidon's Fireplace" },
  { h: "The Dua Brand", n: "Dua's SoHo" },
  { h: "The Dua Brand", n: "Gentleman's Club" },
  { h: "The Dua Brand", n: "True Self Absolute" },
  { h: "The Dua Brand", n: "Poseidon's Cotton Candy" },
  { h: "The Dua Brand", n: "Dua's Water Primal Essence" },
  { h: "The Dua Brand", n: "Rome in Coral Fantasy" },
  { h: "The Dua Brand", n: "Midnight Candy For 1 & Only" },
  { h: "The Dua Brand", n: "1 & Only Jazz Club" },
  { h: "The Dua Brand", n: "Victorian Poseidon" },
  { h: "The Dua Brand", n: "Ottoman Breeze" },
  { h: "The Dua Brand", n: "Ottomans Vert Breeze" },
  { h: "The Dua Brand", n: "Aphrodisiac" },
  { h: "The Dua Brand", n: "Black Widow" },
  { h: "The Dua Brand", n: "City of Dua" },
  { h: "The Dua Brand", n: "Midnight Rendezvous" },
  { h: "The Dua Brand", n: "Gone Swimming" },
  { h: "The Dua Brand", n: "Cola Fizz Of Tonka Beans" },
  { h: "The Dua Brand", n: "Smoky Cuba Tabac" },
  { h: "The Dua Brand", n: "Fortune" },
  { h: "The Dua Brand", n: "Supernova Cologne Intense" },
  { h: "The Dua Brand", n: "Wooden Lucky Charm" },
  { h: "The Dua Brand", n: "Rome in Green" },
  { h: "The Dua Brand", n: "Queen Of The Chess" },
  { h: "The Dua Brand", n: "Midnight Fig" },
  { h: "The Dua Brand", n: "The Mobster vs Poseidon" },
  { h: "The Dua Brand", n: "Aroma of Heaven" },
  { h: "The Dua Brand", n: "Imperialé Matrix" },

  { h: "Maison Alhambra", n: "Black Origami" },
  { h: "Maison Alhambra", n: "Daring Blue For Life" },
  { h: "Maison Alhambra", n: "Glacier Bold" },
  { h: "Maison Alhambra", n: "Glacier Le Noir" },
  { h: "Maison Alhambra", n: "Hercules" },
  { h: "Maison Alhambra", n: "Jean Lowe Immortal" },
  { h: "Maison Alhambra", n: "Jean Lowe Vibe" },
  { h: "Maison Alhambra", n: "Kismet Angel" },
  { h: "Maison Alhambra", n: "Perseus" },
  { h: "Maison Alhambra", n: "Salvo Elixir" },
  { h: "Maison Alhambra", n: "Salvo Intense" },
  { h: "Maison Alhambra", n: "Tobacco Touch" },
  { h: "Maison Alhambra", n: "Victorioso" },
  { h: "Maison Alhambra", n: "Victorioso Myth" },
  { h: "Maison Alhambra", n: "Victorioso Nero" },
  { h: "Maison Alhambra", n: "Yeah Man" },
  { h: "Maison Alhambra", n: "Your Touch Amber" },
  { h: "Maison Alhambra", n: "Your Touch Oud" },
  { h: "Maison Alhambra", n: "Your Touch Sandal" },

  { h: "Lattafa", n: "Opulent Dubai" },
  { h: "Lattafa", n: "Mashrabya" },
  { h: "Lattafa", n: "Ameer Al Oudh Intense" },
  { h: "Lattafa", n: "Vintage Radio" },
  { h: "Lattafa", n: "Najdia Intense" },
  { h: "Lattafa", n: "Qaed Al Fursan" },
  { h: "Lattafa", n: "Hayaati Gold Elixir" },
  { h: "Lattafa", n: "Ana Abiyedh White Musk" },
  { h: "Lattafa", n: "Teriaq Intense" },
  { h: "Lattafa", n: "Liam Grey" },
  { h: "Lattafa", n: "Opulent Musk" },
  { h: "Lattafa", n: "Bade'e Al Oud For Glory" },
  { h: "Lattafa", n: "Bade'e Al Oud Sublime" },
  { h: "Lattafa", n: "Bade'e Al Oud Honor & Glory" },
  { h: "Lattafa", n: "Bade'e Al Oud Amethyst" },
  { h: "Lattafa", n: "Khamrah Qahwa" },

  { h: "Fragrance World", n: "Suits" },
  { h: "Fragrance World", n: "Lust Cherry" },
  { h: "Fragrance World", n: "Intense Peach" },
  { h: "Fragrance World", n: "Oud Wonder" },
  { h: "Fragrance World", n: "Ebony Fume" },
  { h: "Fragrance World", n: "Proud of You" },
  { h: "Fragrance World", n: "Proud of You Absolute" },
  { h: "Fragrance World", n: "La Uno Million Elixir" },
  { h: "Fragrance World", n: "Neroli Riviera" },

  { h: "Banana Republic", n: "Grassland" },
  { h: "Banana Republic", n: "Linen Vetiver" },
  { h: "Banana Republic", n: "Vintage Green 78" },
  { h: "Banana Republic", n: "Tobacco & Tonka Bean" },
  { h: "Banana Republic", n: "Midnight Hour" },
  { h: "Banana Republic", n: "Dark Cherry & Amber" },
  { h: "Banana Republic", n: "Metal Rain" },

  { h: "Designer Originals", n: "Bleu de Chanel EDT" },
  { h: "Designer Originals", n: "Montblanc Legend Spirit" },
  { h: "Designer Originals", n: "Nautica Voyage" },
  { h: "Designer Originals", n: "Dior Sauvage EDP" },
  { h: "Designer Originals", n: "Davidoff Cool Water" },

  { h: "Valencia / Millionaire", n: "Valencia Uomo Intense" },
  { h: "Valencia / Millionaire", n: "Valencia Uomo Fantasy" },
  { h: "Valencia / Millionaire", n: "Valencia Uomo" },
  { h: "Valencia / Millionaire", n: "Millionaire Royal" },

  { h: "Khadlaj", n: "Island Dreams" },
  { h: "Khadlaj", n: "Shiyaaka Snow" },
  { h: "Khadlaj", n: "Shiyaaka Blue" },
  { h: "Khadlaj", n: "Epoque Artistique" },

  { h: "Mod Fragrances", n: "Haven Eclipse" },
  { h: "Mod Fragrances", n: "Secret Embers" },
  { h: "Mod Fragrances", n: "Musk Melody" },
  { h: "Mod Fragrances", n: "Reverence Extrait" },
  { h: "Mod Fragrances", n: "The Exposed Extrait" },

  { h: "Rasasi", n: "Fattan" },
  { h: "Rasasi", n: "Hawas Ice" },
  { h: "Rasasi", n: "Hawas Kobra" },

  { h: "Zimaya", n: "Itqan Noir" },
  { h: "Zimaya", n: "Reverie Aqua Pour Homme" },
  { h: "Zimaya", n: "Abadi Opulent" },

  { h: "Yom & Layl", n: "Wazir" },
  { h: "Yom & Layl", n: "Raazi" },
  { h: "Yom & Layl", n: "Rayees" },
  { h: "Yom & Layl", n: "Nor" },
  { h: "Yom & Layl", n: "Limitless" },
  { h: "Yom & Layl", n: "Gladiator" },

  { h: "Armaf", n: "Le Parfait" },
  { h: "Armaf", n: "Club de Nuit Milestone" },

  { h: "Anti Social Parfum Club", n: "Most Wanted" },
  { h: "Afnan", n: "Supremacy Collector's Edition" },
  { h: "Ajmal", n: "Evoke Gold" },
  { h: "Al Haramain", n: "Detour Noir" },
  { h: "Crown / Al-Rehab", n: "Silver (oil)" },
  { h: "Atralia", n: "Amazonas Avalanche" },
  { h: "French Avenue", n: "Liquid Brun" },
  { h: "Game of Spades", n: "Wildcard" },
  { h: "Grandeur", n: "Iconic Built" },
  { h: "Paris Corner", n: "Rifaqat" },
  { h: "Rayhaan", n: "Aquatica" },
  { h: "Royalty by Maluma", n: "Onyx" },
  { h: "Vurv", n: "Royce Black" },

  { h: "MIRIS Oils", n: "MIRIS No54602" },
  { h: "MIRIS Oils", n: "MIRIS No56902" },
  { h: "MIRIS Oils", n: "MIRIS No47319" },
  { h: "MIRIS Oils", n: "MIRIS No59797" }
];

const HOUSE_NAME_MAP = {
  "His Perspective": "His Perspective Extrait",
  "Imperialé Matrix": "Imperiale Matrix",
  "Opulent Dubai": "Opulent Dubai God of Fire",
  "Shiyaaka Snow": "Shiyaaka Snow",
  "Silver (oil)": "Crown / Al-Rehab Silver",
  "Most Wanted": "Anti Social Parfum Club Most Wanted",
  "Supremacy Collector's Edition": "Afnan Supremacy Collector's Edition",
  "Evoke Gold": "Ajmal Evoke Gold",
  "Detour Noir": "Al Haramain Detour Noir",
  "Amazonas Avalanche": "Atralia Amazonas Avalanche",
  "Liquid Brun": "French Avenue Liquid Brun",
  Wildcard: "Game of Spades Wildcard",
  "Iconic Built": "Grandeur Iconic Built",
  Aquatica: "Rayhaan Aquatica",
  Onyx: "Royalty by Maluma Onyx",
  "Royce Black": "Vurv Royce Black"
};

const COLL = PDF_COLLECTION.map((item) => {
  const lookupName = HOUSE_NAME_MAP[item.n] || item.n;
  const legacy = LEGACY_COLL.find((f) => f.n === lookupName) || LEGACY_COLL.find((f) => f.n === item.n);
  return {
    n: item.n,
    h: item.h,
    d: legacy?.d || [],
    o: legacy?.o || [],
    s: legacy?.s || "All Season",
    r: legacy?.r || 3,
    i: legacy?.i || ""
  };
});

const TABS = ["Home", "Collection", "Wear Today", "Layering", "Display", "Buy or Skip", "Analytics"];
const QUICK_OCCASIONS = ["Travel", "Plane", "Office", "Date", "Casual", "Outdoors", "Evening", "Special Event"];
const ENVIRONMENTS = ["Indoors", "Mixed", "Outdoors"];

const RECOMMENDATION_WEIGHTS = {
  Travel: { fresh: 3, clean: 3, aquatic: 2, citrus: 2, woody: 1, blue: 1 },
  Plane: { clean: 5, fresh: 4, woody: 3, musk: 3, citrus: 2, vetiver: 3, earthy: 2, herbal: 2, mineral: 2, aquatic: 1, blue: 1, floral: 1, amber: -4, tobacco: -5, oud: -5, gourmand: -4, dark: -4, smoky: -5, loud: -4, sweet: -3 },
  Office: { fresh: 3, woody: 2, blue: 2, clean: 2, citrus: 1, aquatic: 1 },
  Date: { amber: 3, woody: 2, sweet: 2, musk: 2, spicy: 2, elegant: 1 },
  Casual: { fresh: 3, aquatic: 2, citrus: 2, green: 2, woody: 1 },
  Outdoors: { fresh: 3, green: 3, aquatic: 2, citrus: 2, marine: 1 },
  Evening: { amber: 3, woody: 2, tobacco: 2, dark: 2, oriental: 2, spicy: 1 },
  SpecialEvent: { elegant: 3, amber: 2, woody: 2, dark: 2, oriental: 2, upscale: 2 }
};

const darkTheme = {
  bg: "#0b0b10",
  panel: "#15151d",
  panelAlt: "#1d1d27",
  text: "#f5f1e8",
  muted: "#a8a3b3",
  accent: "#c9a84c",
  border: "rgba(255,255,255,0.08)",
  accentSoft: "rgba(201,168,76,0.14)"
};

const lightTheme = {
  bg: "#f6f1e8",
  panel: "#ffffff",
  panelAlt: "#f3eee5",
  text: "#1f1a14",
  muted: "#6b6257",
  accent: "#9b6a17",
  border: "rgba(31,26,20,0.12)",
  accentSoft: "rgba(155,106,23,0.12)"
};

function scoreFragrance(f, weights, weather = null, environment = "Mixed") {
  const dnaScore = (f.d || []).reduce((sum, note) => sum + (weights[note] || 0), f.r || 0);
  const temp = weather && typeof weather.temp === "number" ? weather.temp : null;
  const season = (f.s || "").toLowerCase();
  const dna = f.d || [];
  let adjustment = 0;

  if (temp !== null) {
    if (temp <= 55) {
      if (season.includes("fall") || season.includes("winter")) adjustment += 3;
      if (season.includes("spring/summer") || season === "summer" || season === "spring") adjustment -= 3;
      if (dna.includes("fresh")) adjustment -= environment === "Indoors" ? 1 : 3;
      if (dna.includes("aquatic") || dna.includes("marine")) adjustment -= environment === "Indoors" ? 1 : 4;
      if (dna.includes("citrus") || dna.includes("blue")) adjustment -= environment === "Indoors" ? 0 : 2;
      if (dna.includes("amber") || dna.includes("tobacco") || dna.includes("oriental") || dna.includes("dark")) adjustment += environment === "Indoors" ? 1 : 3;
      if (dna.includes("woody") || dna.includes("spicy")) adjustment += 2;
    } else if (temp >= 82) {
      if (season.includes("spring/summer") || season === "summer" || season === "spring") adjustment += 3;
      if (season.includes("fall") || season.includes("winter")) adjustment -= 3;
      if (dna.includes("fresh") || dna.includes("aquatic") || dna.includes("citrus") || dna.includes("green")) adjustment += environment === "Indoors" ? 1 : 3;
      if (dna.includes("amber") || dna.includes("oud") || dna.includes("gourmand") || dna.includes("tobacco")) adjustment += environment === "Indoors" ? -1 : -4;
    } else {
      if (season.includes("all season")) adjustment += 1;
      if (environment === "Indoors" && (dna.includes("clean") || dna.includes("woody") || dna.includes("musk") || dna.includes("vetiver"))) adjustment += 1;
    }
  }

  if (environment === "Indoors") {
    if (dna.includes("clean") || dna.includes("woody") || dna.includes("musk") || dna.includes("vetiver") || dna.includes("fougere")) adjustment += 1;
    if (dna.includes("dark")) adjustment -= 1;
  }

  if (environment === "Outdoors") {
    if (dna.includes("fresh") || dna.includes("aquatic") || dna.includes("citrus") || dna.includes("green")) adjustment += 1;
    if (dna.includes("tobacco") || dna.includes("gourmand")) adjustment -= 1;
  }

  return dnaScore + adjustment;
}

function pickTop(items, weights, count = 3, weather = null, environment = "Mixed") {
  return [...items]
    .map((item) => ({ item, score: scoreFragrance(item, weights, weather, environment) }))
    .sort((a, b) => b.score - a.score)
    .slice(0, count)
    .map((entry) => entry.item);
}

function getSprayAdvice(f, occasion) {
  const dna = f.d || [];
  const rich = dna.some((note) => ["amber", "oud", "tobacco", "oriental", "gourmand", "dark", "sweet"].includes(note));
  const fresh = dna.some((note) => ["fresh", "aquatic", "citrus", "green", "clean", "marine", "blue"].includes(note));
  const elegant = dna.some((note) => ["elegant", "woody", "musk", "vetiver", "fougere"].includes(note));
  const isOil = /oil/i.test(f.h || "") || /oil/i.test(f.n || "");

  if (isOil) {
    if (occasion === "Plane") {
      return rich ? "Apply 1 small dab only, on chest." : "Apply 1 to 2 very small dabs, kept close to skin.";
    }
    if (occasion === "Office") {
      return rich ? "Apply 1 small dab to chest or inner wrist." : "Apply 2 small dabs max, kept controlled.";
    }
    if (occasion === "Date") {
      return rich ? "Apply 2 small dabs, chest and one wrist." : "Apply 2 to 3 small dabs for a closer scent bubble.";
    }
    if (occasion === "Outdoors") {
      return fresh ? "Apply 3 small dabs, since lighter oils sit closer to skin." : "Apply 2 small dabs and reassess.";
    }
    if (occasion === "Evening" || occasion === "Special Event") {
      return rich ? "Apply 2 to 3 small dabs max, kept refined." : "Apply 2 small dabs for a polished presence.";
    }
    return fresh ? "Apply 2 small dabs as a safe default." : "Apply 1 to 2 small dabs as a safe default.";
  }

  if (occasion === "Travel") {
    return rich ? "2 sprays max. One chest, one back of neck." : "3 sprays. One chest, one back of neck, one shirt.";
  }
  if (occasion === "Plane") {
    return rich ? "1 spray max on chest only. Skip shirt and neck for enclosed seating." : "1 to 2 sprays max. Keep it close to skin and avoid overspraying.";
  }
  if (occasion === "Office") {
    if (rich) return "2 sprays. Keep it controlled for office wear.";
    if (fresh || elegant) return "3 sprays. Chest, neck, and one light shirt spray.";
  }
  if (occasion === "Date") {
    return rich ? "4 sprays. Chest, both sides of neck, one wrist." : "3 sprays. Chest and neck for a closer scent bubble.";
  }
  if (occasion === "Casual") {
    return fresh ? "4 sprays. Easy, relaxed, and versatile." : "3 sprays. Enough presence without overdoing it.";
  }
  if (occasion === "Outdoors") {
    return fresh ? "5 sprays. Fresh scents can handle more air and movement." : "4 sprays. Give it room to project outside.";
  }
  if (occasion === "Evening") {
    return rich ? "4 sprays. Stronger presence for nighttime." : "3 sprays. Polished without getting too loud.";
  }
  if (occasion === "Special Event") {
    return rich ? "4 to 5 sprays. Make it noticeable but refined." : "4 sprays. Enough lift for a dressed-up setting.";
  }
  return "3 sprays as a safe default.";
}

function getWhyWear(f, occasion, weather) {
  const dna = f.d || [];
  const reasons = [];

  if (["Office", "Travel", "Plane"].includes(occasion)) {
    if (dna.includes("fresh") || dna.includes("clean") || dna.includes("blue")) reasons.push("it smells clean, polished, and easy to wear");
    if (dna.includes("woody") || dna.includes("vetiver") || dna.includes("fougere")) reasons.push("it keeps a professional edge without feeling boring");
    if (occasion === "Plane") reasons.push("it stays more contained and low-risk for enclosed seating");
  }

  if (occasion === "Date") {
    if (dna.includes("amber") || dna.includes("musk") || dna.includes("sweet")) reasons.push("it has warmth and closeness that works well for date settings");
    if (dna.includes("woody") || dna.includes("elegant")) reasons.push("it feels intentional and put together");
  }

  if (["Casual", "Outdoors"].includes(occasion)) {
    if (dna.includes("fresh") || dna.includes("aquatic") || dna.includes("green") || dna.includes("citrus")) reasons.push("it feels energetic and effortless");
    if (dna.includes("marine") || dna.includes("clean")) reasons.push("it stays easygoing and comfortable in open air");
  }

  if (["Evening", "Special Event"].includes(occasion)) {
    if (dna.includes("amber") || dna.includes("tobacco") || dna.includes("oriental") || dna.includes("dark")) reasons.push("it has the depth and presence that suits nighttime wear");
    if (dna.includes("elegant") || dna.includes("woody") || dna.includes("upscale")) reasons.push("it comes across dressed up and confident");
  }

  if (!reasons.length) {
    if (f.r >= 4) reasons.push("it is one of the stronger overall fits in your collection for this use");
    if (f.i) reasons.push(`the style leans ${f.i}, which makes it easy to place and wear`);
  }

  const mainReason = reasons.length ? reasons[0].charAt(0).toUpperCase() + reasons[0].slice(1) + "." : "It fits the occasion well and sits in a strong lane within your collection.";
  return mainReason + " " + getWeatherWhy(weather);
}

function getAdvisorLabel(index) {
  if (index === 0) return "Safest pick";
  if (index === 1) return "Most unique";
  return "Best performer";
}

function isPlaneSafeFragrance(f) {
  const dna = f.d || [];
  const season = (f.s || "").toLowerCase();
  const occasions = f.o || [];

  const tooHeavy = dna.some((note) => ["oud", "tobacco", "gourmand", "dark", "smoky", "loud", "club", "bold", "statement", "oriental"].includes(note));
  const tooSweet = dna.filter((note) => ["sweet", "amber", "vanilla", "boozy"].includes(note)).length >= 2;
  const winterHeavy = (season.includes("fall") || season.includes("winter")) && !dna.includes("clean") && !dna.includes("musk") && !dna.includes("vetiver") && !dna.includes("fresh") && !dna.includes("earthy") && !dna.includes("herbal") && !dna.includes("mineral");
  const softEnough = dna.some((note) => ["clean", "musk", "woody", "vetiver", "fresh", "citrus", "blue", "aquatic", "light", "airy", "office", "earthy", "herbal", "mineral"].includes(note));
  const officeLike = occasions.some((o) => ["Office", "Casual", "daytime", "Versatile", "Smart casual"].includes(o));
  const explicitlyNight = occasions.some((o) => ["Evening", "Special Event", "Night out / clubbing", "Night out", "clubbing", "Formal"].includes(o));

  if (tooHeavy) return false;
  if (tooSweet && !dna.includes("clean") && !dna.includes("musk") && !dna.includes("vetiver") && !dna.includes("earthy")) return false;
  if (winterHeavy) return false;
  if (explicitlyNight && !officeLike) return false;
  return softEnough || officeLike;
}

function getPlaneSafePicks(items, weather, environment, count = 5) {
  const safePool = items.filter((f) => isPlaneSafeFragrance(f));
  const pool = safePool.length
    ? safePool
    : items.filter((f) => (f.d || []).some((note) => ["clean", "musk", "woody", "vetiver", "fresh", "citrus", "blue", "aquatic", "earthy", "herbal", "mineral"].includes(note)));
  const weights = getWeatherAdjustedWeights(RECOMMENDATION_WEIGHTS.Plane || {}, weather);
  return pickTop(pool, weights, count, weather, environment);
}

function getWeatherAdjustedWeights(baseWeights, weather) {
  const adjusted = { ...baseWeights };
  const temp = weather && typeof weather.temp === "number" ? weather.temp : null;
  const label = getWeatherLabel(weather && weather.code);
  const windy = weather && typeof weather.wind === "number" && weather.wind >= 12;

  if (temp !== null) {
    if (temp >= 78) {
      adjusted.fresh = (adjusted.fresh || 0) + 2;
      adjusted.aquatic = (adjusted.aquatic || 0) + 2;
      adjusted.citrus = (adjusted.citrus || 0) + 2;
      adjusted.green = (adjusted.green || 0) + 1;
      adjusted.clean = (adjusted.clean || 0) + 1;
      adjusted.amber = (adjusted.amber || 0) - 1;
      adjusted.gourmand = (adjusted.gourmand || 0) - 2;
      adjusted.oud = (adjusted.oud || 0) - 2;
      adjusted.tobacco = (adjusted.tobacco || 0) - 2;
      adjusted.dark = (adjusted.dark || 0) - 1;
    } else if (temp <= 52) {
      adjusted.amber = (adjusted.amber || 0) + 2;
      adjusted.woody = (adjusted.woody || 0) + 1;
      adjusted.oriental = (adjusted.oriental || 0) + 2;
      adjusted.tobacco = (adjusted.tobacco || 0) + 2;
      adjusted.gourmand = (adjusted.gourmand || 0) + 1;
      adjusted.dark = (adjusted.dark || 0) + 1;
      adjusted.fresh = (adjusted.fresh || 0) - 1;
      adjusted.aquatic = (adjusted.aquatic || 0) - 1;
      adjusted.citrus = (adjusted.citrus || 0) - 1;
    }
  }

  if (["Rain", "Showers", "Thunderstorm", "Drizzle", "Fog"].includes(label)) {
    adjusted.woody = (adjusted.woody || 0) + 1;
    adjusted.vetiver = (adjusted.vetiver || 0) + 1;
    adjusted.clean = (adjusted.clean || 0) + 1;
    adjusted.ozonic = (adjusted.ozonic || 0) + 1;
    adjusted.gourmand = (adjusted.gourmand || 0) - 1;
  }

  if (windy) {
    adjusted.intense = (adjusted.intense || 0) + 1;
    adjusted.woody = (adjusted.woody || 0) + 1;
    adjusted.amber = (adjusted.amber || 0) + 1;
    adjusted.fresh = (adjusted.fresh || 0) + 1;
  }

  return adjusted;
}

function getWeatherWhy(weather) {
  const temp = weather && typeof weather.temp === "number" ? Math.round(weather.temp) : null;
  const label = getWeatherLabel(weather && weather.code);
  if (temp === null) return "It matches the occasion well.";
  if (temp >= 78) return "The warm weather favors lighter, fresher scents that stay easy to wear.";
  if (temp <= 52) return "The cooler weather supports deeper scents with more warmth and body.";
  if (["Rain", "Showers", "Thunderstorm", "Drizzle", "Fog"].includes(label)) return "The current conditions lean well with clean woods, vetiver, and fresher structure.";
  return "The current weather keeps this in a comfortable, wearable lane.";
}

function Badge({ children, theme }) {
  return (
    <span
      className="inline-flex items-center rounded-full border px-2 py-0.5 text-xs"
      style={{ borderColor: theme.border, color: theme.muted }}
    >
      {children}
    </span>
  );
}

function ThemeToggle({ mode, setMode, theme }) {
  return (
    <div className="inline-flex rounded-2xl border p-1" style={{ borderColor: theme.border, background: theme.panel }}>
      <button
        onClick={() => setMode("dark")}
        className="rounded-xl px-3 py-2 text-sm"
        style={{
          background: mode === "dark" ? theme.accentSoft : "transparent",
          color: mode === "dark" ? theme.text : theme.muted
        }}
      >
        Dark
      </button>
      <button
        onClick={() => setMode("light")}
        className="rounded-xl px-3 py-2 text-sm"
        style={{
          background: mode === "light" ? theme.accentSoft : "transparent",
          color: mode === "light" ? theme.text : theme.muted
        }}
      >
        Light
      </button>
    </div>
  );
}

function Card({ children, theme }) {
  return (
    <div
      className="rounded-3xl border p-4 shadow-sm"
      style={{ background: theme.panel, borderColor: theme.border }}
    >
      {children}
    </div>
  );
}

function getWeatherLabel(code) {
  if (code === 0) return "Clear";
  if ([1, 2, 3].includes(code)) return "Partly cloudy";
  if ([45, 48].includes(code)) return "Fog";
  if ([51, 53, 55, 56, 57].includes(code)) return "Drizzle";
  if ([61, 63, 65, 66, 67].includes(code)) return "Rain";
  if ([71, 73, 75, 77].includes(code)) return "Snow";
  if ([80, 81, 82].includes(code)) return "Showers";
  if ([95, 96, 99].includes(code)) return "Thunderstorm";
  return "Current conditions";
}

function useWeatherPreference() {
  const [locationMode, setLocationMode] = useState(() => {
    try {
      return localStorage.getItem("vault_location_mode") || "auto";
    } catch (e) {
      return "auto";
    }
  });
  const [manualCity, setManualCity] = useState(() => {
    try {
      return localStorage.getItem("vault_manual_city") || "";
    } catch (e) {
      return "";
    }
  });
  const [weather, setWeather] = useState({ loading: true, error: "", temp: null, wind: null, code: null, city: "Your area" });

  useEffect(() => {
    try {
      localStorage.setItem("vault_location_mode", locationMode);
      localStorage.setItem("vault_manual_city", manualCity);
    } catch (e) {}
  }, [locationMode, manualCity]);

  useEffect(() => {
    let cancelled = false;

    const applyWeather = (payload) => {
      if (!cancelled) setWeather(payload);
    };

    async function loadWeatherByCoords(lat, lon, labelOverride = "") {
      try {
        const weatherRes = await fetch(
          "https://api.open-meteo.com/v1/forecast?latitude=" + lat + "&longitude=" + lon + "&current=temperature_2m,weather_code,wind_speed_10m&temperature_unit=fahrenheit&wind_speed_unit=mph"
        );
        const weatherData = await weatherRes.json();

        let city = labelOverride || "Your area";
        if (!labelOverride) {
          try {
            const geoRes = await fetch(
              "https://geocoding-api.open-meteo.com/v1/reverse?latitude=" + lat + "&longitude=" + lon + "&language=en&format=json"
            );
            const geoData = await geoRes.json();
            if (geoData && geoData.results && geoData.results[0]) {
              city = geoData.results[0].name || geoData.results[0].admin1 || city;
            }
          } catch (e) {}
        }

        applyWeather({
          loading: false,
          error: "",
          temp: weatherData && weatherData.current ? weatherData.current.temperature_2m : null,
          wind: weatherData && weatherData.current ? weatherData.current.wind_speed_10m : null,
          code: weatherData && weatherData.current ? weatherData.current.weather_code : null,
          city
        });
      } catch (e) {
        applyWeather({
          loading: false,
          error: "Unable to load weather.",
          temp: null,
          wind: null,
          code: null,
          city: labelOverride || "Your area"
        });
      }
    }

    async function loadWeatherByCity(city) {
      try {
        const geoRes = await fetch(
          "https://geocoding-api.open-meteo.com/v1/search?name=" + encodeURIComponent(city) + "&count=1&language=en&format=json"
        );
        const geoData = await geoRes.json();
        if (geoData && geoData.results && geoData.results[0]) {
          const result = geoData.results[0];
          const label = result.name + (result.admin1 ? ", " + result.admin1 : "");
          loadWeatherByCoords(result.latitude, result.longitude, label);
        } else {
          applyWeather({
            loading: false,
            error: "City not found.",
            temp: null,
            wind: null,
            code: null,
            city: city || "Manual city"
          });
        }
      } catch (e) {
        applyWeather({
          loading: false,
          error: "Unable to load weather.",
          temp: null,
          wind: null,
          code: null,
          city: city || "Manual city"
        });
      }
    }

    applyWeather({
      loading: true,
      error: "",
      temp: null,
      wind: null,
      code: null,
      city: locationMode === "manual" ? (manualCity || "Manual city") : "Your area"
    });

    if (locationMode === "manual" && manualCity.trim()) {
      loadWeatherByCity(manualCity.trim());
    } else if (typeof navigator !== "undefined" && navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        (position) => loadWeatherByCoords(position.coords.latitude, position.coords.longitude),
        () => loadWeatherByCoords(38.8816, -77.091, "Arlington area"),
        { enableHighAccuracy: false, timeout: 8000, maximumAge: 300000 }
      );
    } else {
      loadWeatherByCoords(38.8816, -77.091, "Arlington area");
    }

    return () => {
      cancelled = true;
    };
  }, [locationMode, manualCity]);

  return { weather, locationMode, setLocationMode, manualCity, setManualCity };
}

function HomeTab({ collection, theme }) {
  const [occasion, setOccasion] = useState("Office");
  const [environment, setEnvironment] = useState("Mixed");
  const { weather, locationMode, setLocationMode, manualCity, setManualCity } = useWeatherPreference();

  const filteredForOccasion = useMemo(() => {
    return collection.filter((f) => (f.o || []).includes(occasion));
  }, [collection, occasion]);

  const quickPicks = useMemo(() => {
    const pool = filteredForOccasion.length ? filteredForOccasion : collection;
    if (occasion === "Plane") {
      return getPlaneSafePicks(pool, weather, environment, 3);
    }
    const weightKey = occasion === "Special Event" ? "SpecialEvent" : occasion;
    const weatherWeights = getWeatherAdjustedWeights(RECOMMENDATION_WEIGHTS[weightKey] || {}, weather);
    return pickTop(pool, weatherWeights, 3, weather, environment);
  }, [collection, filteredForOccasion, occasion, weather, environment]);

  const topFresh = useMemo(() => {
    return pickTop(collection, { fresh: 3, aquatic: 3, citrus: 2, blue: 2, green: 2 }, 3, weather, environment);
  }, [collection, weather, environment]);

  const topEvening = useMemo(() => {
    return pickTop(collection, { amber: 3, gourmand: 3, woody: 2, tobacco: 2, oriental: 2 }, 3, weather, environment);
  }, [collection, weather, environment]);

  return (
    <div className="grid gap-4">
      <Card theme={theme}>
        <div className="flex flex-col gap-3 md:flex-row md:items-center md:justify-between">
          <div>
            <div className="text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>
              Live weather
            </div>
            <div className="text-sm" style={{ color: theme.muted }}>
              {weather.loading ? "Loading current conditions..." : weather.error ? weather.error : weather.city + " • " + getWeatherLabel(weather.code)}
            </div>
            <div className="mt-3 grid gap-3 md:grid-cols-2">
              <div>
                <label className="mb-2 block text-xs uppercase tracking-[0.16em]" style={{ color: theme.muted }}>
                  Weather source
                </label>
                <select
                  value={locationMode}
                  onChange={(e) => setLocationMode(e.target.value)}
                  className="w-full rounded-2xl border px-4 py-3 outline-none"
                  style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }}
                >
                  <option value="auto">Use current location</option>
                  <option value="manual">Use manual city</option>
                </select>
              </div>
              {locationMode === "manual" ? (
                <div>
                  <label className="mb-2 block text-xs uppercase tracking-[0.16em]" style={{ color: theme.muted }}>
                    Manual city
                  </label>
                  <input
                    value={manualCity}
                    onChange={(e) => setManualCity(e.target.value)}
                    placeholder="Example: Chicago or 60601"
                    className="w-full rounded-2xl border px-4 py-3 outline-none"
                    style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }}
                  />
                </div>
              ) : null}
            </div>
          </div>
          {!weather.loading && !weather.error ? (
            <div className="grid grid-cols-3 gap-3 md:min-w-[360px]">
              <div className="rounded-2xl p-3 text-center" style={{ background: theme.panelAlt }}>
                <div className="text-xs uppercase tracking-[0.16em]" style={{ color: theme.muted }}>Temp</div>
                <div className="mt-1 text-xl font-semibold" style={{ color: theme.text }}>{weather.temp !== null ? Math.round(weather.temp) : "--"}°F</div>
              </div>
              <div className="rounded-2xl p-3 text-center" style={{ background: theme.panelAlt }}>
                <div className="text-xs uppercase tracking-[0.16em]" style={{ color: theme.muted }}>Wind</div>
                <div className="mt-1 text-xl font-semibold" style={{ color: theme.text }}>{weather.wind !== null ? Math.round(weather.wind) : "--"} mph</div>
              </div>
              <div className="rounded-2xl p-3 text-center" style={{ background: theme.panelAlt }}>
                <div className="text-xs uppercase tracking-[0.16em]" style={{ color: theme.muted }}>Condition</div>
                <div className="mt-1 text-sm font-semibold" style={{ color: theme.text }}>{getWeatherLabel(weather.code)}</div>
              </div>
            </div>
          ) : null}
        </div>
      </Card>

      <Card theme={theme}>
        <div className="mb-4 flex flex-col gap-3 md:flex-row md:items-end md:justify-between">
          <div>
            <div className="text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>
              Quick recommendation
            </div>
            <div className="text-sm" style={{ color: theme.muted }}>
              Pick an occasion and get weather-aware, collection-first recommendations.
            </div>
          </div>
          <div className="grid gap-3 md:grid-cols-2 md:min-w-[520px]">
            <div>
              <label className="mb-2 block text-xs uppercase tracking-[0.18em]" style={{ color: theme.muted }}>
                Occasion
              </label>
              <select
                value={occasion}
                onChange={(e) => setOccasion(e.target.value)}
                className="w-full rounded-2xl border px-4 py-3 outline-none"
                style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }}
              >
                {QUICK_OCCASIONS.map((item) => (
                  <option key={item} value={item}>{item}</option>
                ))}
              </select>
            </div>
            <div>
              <label className="mb-2 block text-xs uppercase tracking-[0.18em]" style={{ color: theme.muted }}>
                Environment
              </label>
              <select
                value={environment}
                onChange={(e) => setEnvironment(e.target.value)}
                className="w-full rounded-2xl border px-4 py-3 outline-none"
                style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }}
              >
                {ENVIRONMENTS.map((item) => (
                  <option key={item} value={item}>{item}</option>
                ))}
              </select>
            </div>
          </div>
        </div>

        <div className="grid gap-3 md:grid-cols-3">
          {quickPicks.map((f, index) => (
            <div key={f.n} className="rounded-2xl p-4" style={{ background: theme.panelAlt }}>
              <div className="mb-2 text-xs uppercase tracking-[0.18em]" style={{ color: theme.accent }}>{getAdvisorLabel(index)}</div>
              <div className="font-semibold" style={{ color: theme.text }}>{f.n}</div>
              <div className="text-sm" style={{ color: theme.muted }}>{f.h}</div>
              <div className="mt-2 flex flex-wrap gap-2">
                {(f.d || []).map((note) => (
                  <Badge key={note} theme={theme}>{note}</Badge>
                ))}
              </div>
              <div className="mt-3 text-sm" style={{ color: theme.text }}><span style={{ color: theme.accent }}>Why wear it:</span> {getWhyWear(f, occasion, weather)}</div>
              <div className="mt-2 text-sm" style={{ color: theme.text }}><span style={{ color: theme.accent }}>Sprays:</span> {getSprayAdvice(f, occasion)}</div>
              <div className="mt-2 text-sm" style={{ color: theme.muted }}>Occasion: {(f.o || []).join(", ")}</div>
              <div className="text-sm" style={{ color: theme.muted }}>Season: {f.s}</div>
              <div className="text-sm" style={{ color: theme.muted }}>Inspired by / style: {f.i || "Original"}</div>
            </div>
          ))}
        </div>
      </Card>

      <div className="grid gap-4 md:grid-cols-2">
        <Card theme={theme}>
          <div className="mb-3 text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>Fresh rotation</div>
          <div className="space-y-3">
            {topFresh.map((f) => (
              <div key={f.n} className="rounded-2xl p-3" style={{ background: theme.panelAlt }}>
                <div className="font-semibold" style={{ color: theme.text }}>{f.n}</div>
                <div className="text-sm" style={{ color: theme.muted }}>{f.h}</div>
                <div className="mt-2 flex flex-wrap gap-2">
                  {(f.d || []).map((note) => (
                    <Badge key={note} theme={theme}>{note}</Badge>
                  ))}
                </div>
                <div className="mt-2 text-sm" style={{ color: theme.muted }}>Inspired by / style: {f.i || "Original"}</div>
              </div>
            ))}
          </div>
        </Card>

        <Card theme={theme}>
          <div className="mb-3 text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>Evening rotation</div>
          <div className="space-y-3">
            {topEvening.map((f) => (
              <div key={f.n} className="rounded-2xl p-3" style={{ background: theme.panelAlt }}>
                <div className="font-semibold" style={{ color: theme.text }}>{f.n}</div>
                <div className="text-sm" style={{ color: theme.muted }}>{f.h}</div>
                <div className="mt-2 flex flex-wrap gap-2">
                  {(f.d || []).map((note) => (
                    <Badge key={note} theme={theme}>{note}</Badge>
                  ))}
                </div>
                <div className="mt-2 text-sm" style={{ color: theme.muted }}>Inspired by / style: {f.i || "Original"}</div>
              </div>
            ))}
          </div>
        </Card>
      </div>
    </div>
  );
}

function CollectionTab({ collection, query, setQuery, theme }) {
  const [view, setView] = useState("House");
  const [selectedFragrance, setSelectedFragrance] = useState(null);
  const [selectedGroup, setSelectedGroup] = useState("");

  const filtered = useMemo(() => {
    const q = query.trim().toLowerCase();
    if (!q) return collection;
    return collection.filter((f) =>
      [f.n, f.h, f.i, ...(f.d || []), ...(f.o || [])]
        .filter(Boolean)
        .some((value) => value.toLowerCase().includes(q))
    );
  }, [collection, query]);

  const collectionViews = ["House", "DNA", "Inspiration", "Occasion", "Plane"];

  const grouped = useMemo(() => {
    const map = new Map();

    if (view === "House") {
      filtered.forEach((f) => {
        const key = f.h || "Unknown House";
        if (!map.has(key)) map.set(key, []);
        map.get(key).push(f);
      });
    }

    if (view === "DNA") {
      filtered.forEach((f) => {
        const notes = (f.d || []).length ? f.d : ["Uncategorized"];
        notes.forEach((note) => {
          const key = note;
          if (!map.has(key)) map.set(key, []);
          map.get(key).push(f);
        });
      });
    }

    if (view === "Inspiration") {
      filtered.forEach((f) => {
        const key = f.i && f.i.trim() ? f.i : "Original / Unspecified";
        if (!map.has(key)) map.set(key, []);
        map.get(key).push(f);
      });
    }

    if (view === "Occasion") {
      filtered.forEach((f) => {
        const occasions = (f.o || []).length ? f.o : ["Unspecified"];
        occasions.forEach((occasion) => {
          const key = occasion;
          if (!map.has(key)) map.set(key, []);
          map.get(key).push(f);
        });
      });
    }

    if (view === "Plane") {
      const planeWeights = getWeatherAdjustedWeights(RECOMMENDATION_WEIGHTS.Plane || {}, { temp: 70, wind: 0, code: 1 });
      const planeSafe = filtered
        .filter((f) => isPlaneSafeFragrance(f))
        .map((f) => ({
          item: f,
          score: scoreFragrance(f, planeWeights, { temp: 70, wind: 0, code: 1 }, "Indoors"),
          sprays: getSprayAdvice(f, "Plane")
        }))
        .sort((a, b) => b.score - a.score)
        .slice(0, 40);

      map.set("Plane Safe", planeSafe.map((entry) => ({ ...entry.item, planeScore: entry.score, planeSprays: entry.sprays })));
    }

    return [...map.entries()]
      .map(([key, items]) => [key, [...items].sort((a, b) => a.n.localeCompare(b.n))])
      .sort((a, b) => a[0].localeCompare(b[0]));
  }, [filtered, view]);

  useEffect(() => {
    if (!grouped.length) {
      setSelectedGroup("");
      return;
    }
    if (!selectedGroup) {
      return;
    }
    const hasSelected = grouped.some(([groupName]) => groupName === selectedGroup);
    if (!hasSelected) setSelectedGroup("");
  }, [grouped, selectedGroup]);

  const visibleGrouped = useMemo(() => {
    if (!selectedGroup) return grouped;
    return grouped.filter(([groupName]) => groupName === selectedGroup);
  }, [grouped, selectedGroup]);

  return (
    <>
      <Card theme={theme}>
        <div className="mb-4 flex flex-col gap-3">
          <div className="flex items-center justify-between gap-3">
            <div>
              <div className="text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>
                Collection
              </div>
              <div style={{ color: theme.muted }}>{filtered.length} shown</div>
            </div>
            <input
              value={query}
              onChange={(e) => setQuery(e.target.value)}
              placeholder="Search fragrance, house, DNA, inspiration"
              className="w-full max-w-md rounded-2xl border px-4 py-2 outline-none"
              style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }}
            />
          </div>

          <div className="flex flex-wrap gap-2">
            {collectionViews.map((name) => {
              const active = view === name;
              return (
                <button
                  key={name}
                  onClick={() => {
                    setView(name);
                    setSelectedGroup("");
                  }}
                  className="rounded-2xl border px-4 py-2 text-sm"
                  style={{
                    borderColor: active ? theme.accent : theme.border,
                    background: active ? theme.accentSoft : theme.panelAlt,
                    color: active ? theme.text : theme.muted,
                  }}
                >
                  {name}
                </button>
              );
            })}
          </div>

          <div className="grid gap-3 md:grid-cols-2">
            <div>
              <label className="mb-2 block text-xs uppercase tracking-[0.18em]" style={{ color: theme.muted }}>
                Jump to {view}
              </label>
              <select
                value={selectedGroup}
                onChange={(e) => setSelectedGroup(e.target.value)}
                className="w-full rounded-2xl border px-4 py-3 outline-none"
                style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }}
              >
                <option value="">All</option>
                {grouped.map(([groupName]) => (
                  <option key={groupName} value={groupName}>{groupName}</option>
                ))}
              </select>
            </div>
            <div className="rounded-2xl border px-4 py-3" style={{ background: theme.panelAlt, borderColor: theme.border }}>
              <div className="text-xs uppercase tracking-[0.16em]" style={{ color: theme.muted }}>Current group</div>
              <div className="mt-1 text-sm" style={{ color: theme.text }}>{selectedGroup || "All"}</div>
            </div>
          </div>
        </div>

        <div className="space-y-5">
          {visibleGrouped.map(([groupName, items]) => (
            <div key={groupName}>
              <div className="mb-3 flex items-center justify-between border-b pb-2" style={{ borderColor: theme.border }}>
                <div className="text-sm font-semibold uppercase tracking-[0.16em]" style={{ color: theme.accent }}>
                  {groupName}
                </div>
                <div className="text-sm" style={{ color: theme.muted }}>{items.length}</div>
              </div>

              <div className="grid gap-3 md:grid-cols-2 xl:grid-cols-3">
                {items.map((f) => (
                  <button
                    key={`${groupName}-${f.n}`}
                    onClick={() => setSelectedFragrance(f)}
                    className="rounded-2xl border p-4 text-left transition hover:scale-[1.01]"
                    style={{ borderColor: theme.border, background: theme.panelAlt }}
                  >
                    <div className="font-semibold" style={{ color: theme.text }}>{f.n}</div>
                    <div className="text-sm" style={{ color: theme.muted }}>{f.h}</div>
                    <div className="mt-2 flex flex-wrap gap-2">
                      {(f.d || []).map((note) => (
                        <Badge key={note} theme={theme}>{note}</Badge>
                      ))}
                    </div>
                    <div className="mt-3 text-sm" style={{ color: theme.muted }}>Season: {f.s}</div>
                    <div className="text-sm" style={{ color: theme.muted }}>Occasion: {(f.o || []).join(", ")}</div>
                    <div className="text-sm" style={{ color: theme.muted }}>Inspired by / style: {f.i || "Original"}</div>
                    {view === "Plane" ? (
                      <div className="mt-1 text-sm" style={{ color: theme.text }}>Plane sprays: {f.planeSprays || getSprayAdvice(f, "Plane")}</div>
                    ) : null}
                  </button>
                ))}
              </div>
            </div>
          ))}
        </div>
      </Card>

      {selectedFragrance ? (
        <div className="fixed inset-0 z-50 flex items-center justify-center bg-black/50 p-4" onClick={() => setSelectedFragrance(null)}>
          <div
            className="max-h-[85vh] w-full max-w-2xl overflow-auto rounded-3xl border p-6"
            style={{ background: theme.panel, borderColor: theme.border }}
            onClick={(e) => e.stopPropagation()}
          >
            <div className="mb-4 flex items-start justify-between gap-4">
              <div>
                <div className="text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>
                  Fragrance details
                </div>
                <h2 className="mt-2 text-2xl font-semibold" style={{ color: theme.text }}>{selectedFragrance.n}</h2>
                <div className="text-sm" style={{ color: theme.muted }}>{selectedFragrance.h}</div>
              </div>
              <button
                onClick={() => setSelectedFragrance(null)}
                className="rounded-2xl border px-4 py-2 text-sm"
                style={{ borderColor: theme.border, color: theme.muted }}
              >
                Close
              </button>
            </div>

            <div className="grid gap-4 md:grid-cols-2">
              <div className="rounded-2xl p-4" style={{ background: theme.panelAlt }}>
                <div className="text-xs uppercase tracking-[0.18em]" style={{ color: theme.accent }}>Inspired by / style</div>
                <div className="mt-2 text-sm" style={{ color: theme.text }}>{selectedFragrance.i || "Original"}</div>
              </div>
              <div className="rounded-2xl p-4" style={{ background: theme.panelAlt }}>
                <div className="text-xs uppercase tracking-[0.18em]" style={{ color: theme.accent }}>Season</div>
                <div className="mt-2 text-sm" style={{ color: theme.text }}>{selectedFragrance.s || "Unspecified"}</div>
              </div>
              <div className="rounded-2xl p-4" style={{ background: theme.panelAlt }}>
                <div className="text-xs uppercase tracking-[0.18em]" style={{ color: theme.accent }}>DNA Tags</div>
                <div className="mt-3 flex flex-wrap gap-2">
                  {(selectedFragrance.d || []).length ? (
                    (selectedFragrance.d || []).map((note) => <Badge key={note} theme={theme}>{note}</Badge>)
                  ) : (
                    <span style={{ color: theme.muted }}>No DNA tags saved.</span>
                  )}
                </div>
              </div>
              <div className="rounded-2xl p-4" style={{ background: theme.panelAlt }}>
                <div className="text-xs uppercase tracking-[0.18em]" style={{ color: theme.accent }}>Occasions</div>
                <div className="mt-2 text-sm" style={{ color: theme.text }}>{(selectedFragrance.o || []).length ? selectedFragrance.o.join(", ") : "Unspecified"}</div>
                {view === "Plane" ? (
                  <div className="mt-2 text-sm" style={{ color: theme.text }}>Plane sprays: {selectedFragrance.planeSprays || getSprayAdvice(selectedFragrance, "Plane")}</div>
                ) : null}
              </div>
              <div className="rounded-2xl p-4 md:col-span-2" style={{ background: theme.panelAlt }}>
                <div className="text-xs uppercase tracking-[0.18em]" style={{ color: theme.accent }}>Collection fit</div>
                <div className="mt-2 text-sm" style={{ color: theme.text }}>
                  {selectedFragrance.n} sits under {selectedFragrance.h}. It is tagged for {selectedFragrance.s || "unspecified seasons"}
                  {selectedFragrance.o && selectedFragrance.o.length ? ` and is positioned for ${selectedFragrance.o.join(", ")}.` : "."}
                  {selectedFragrance.i ? ` Its style reference is ${selectedFragrance.i}.` : ""}
                </div>
              </div>
            </div>
          </div>
        </div>
      ) : null}
    </>
  );
}

function getLayeringAnalysis(base, top) {
  if (!base || !top) {
    return { score: 0, verdict: "Pick two fragrances to analyze.", why: "", ratio: "", sprays: "" };
  }

  const baseDNA = base.d || [];
  const topDNA = top.d || [];
  const shared = baseDNA.filter((note) => topDNA.includes(note));
  const baseOnly = baseDNA.filter((note) => !topDNA.includes(note));
  const topOnly = topDNA.filter((note) => !baseDNA.includes(note));

  const contrastingPairs = [
    ["fresh", "amber"],
    ["aquatic", "woody"],
    ["green", "woody"],
    ["citrus", "musk"],
    ["blue", "amber"],
    ["tobacco", "vanilla"],
    ["oud", "amber"],
    ["vetiver", "citrus"]
  ];

  let contrastBonus = 0;
  const contrastHits = [];
  contrastingPairs.forEach(([a, b]) => {
    const matches = (baseDNA.includes(a) && topDNA.includes(b)) || (baseDNA.includes(b) && topDNA.includes(a));
    if (matches) {
      contrastBonus += 1;
      contrastHits.push(`${a} with ${b}`);
    }
  });

  let clashPenalty = 0;
  const clashHits = [];
  const clashPairs = [["gourmand", "marine"], ["smoky", "aquatic"], ["oud", "airy"], ["dark", "sporty"]];
  clashPairs.forEach(([a, b]) => {
    const clashes = (baseDNA.includes(a) && topDNA.includes(b)) || (baseDNA.includes(b) && topDNA.includes(a));
    if (clashes) {
      clashPenalty += 2;
      clashHits.push(`${a} against ${b}`);
    }
  });

  const rawScore = 5 + shared.length * 1.5 + contrastBonus - clashPenalty;
  const score = Math.max(1, Math.min(10, Math.round(rawScore)));

  let verdict = "Solid combo";
  if (score >= 9) verdict = "Excellent layering match";
  else if (score >= 7) verdict = "Very good layering match";
  else if (score >= 5) verdict = "Borderline combo";
  else verdict = "Do not layer these";

  let ratio = "2 sprays base + 1 spray top";
  if (score >= 8) ratio = "2 sprays base + 2 sprays top";
  if (score >= 5 && score <= 6) ratio = "2 sprays base + 1 light spray top";
  if (score <= 4) ratio = "Not recommended";

  let sprays = "Apply base to chest and neck, then add the top fragrance lightly on neck or shirt.";
  if (score >= 8) sprays = "Apply the base first on chest and one side of neck, then the top on the other side of neck and shirt for lift.";
  if (score >= 5 && score <= 6) sprays = "Test lightly first. Keep the top fragrance controlled so it does not overpower the base.";
  if (score <= 4) sprays = "Skip this layering combo. Wear one of them on its own instead.";

  const whyParts = [];

  if (score >= 8) {
    whyParts.push("This pairing is strong because the two fragrances share enough core structure to smell connected on skin rather than stacked randomly.");
  } else if (score >= 5) {
    whyParts.push("This pairing can work, but it depends on keeping the balance controlled so one side does not blur or overpower the other.");
  } else {
    whyParts.push("These two are not a good layering match, because the scent structures pull in different directions instead of building one clear result.");
  }

  if (shared.length) {
    whyParts.push(`They overlap on ${shared.join(", ")}, which gives the blend a shared backbone and helps the transition from base to top smell smoother.`);
  } else {
    whyParts.push("They do not share much overlap in DNA, so the blend relies almost entirely on contrast instead of cohesion.");
  }

  if (baseOnly.length) {
    whyParts.push(`The base contributes ${baseOnly.slice(0, 4).join(", ")}${baseOnly.length > 4 ? ", and more" : ""}, which is what gives the combo its body, depth, and staying power.`);
  }

  if (topOnly.length) {
    whyParts.push(`The top adds ${topOnly.slice(0, 4).join(", ")}${topOnly.length > 4 ? ", and more" : ""}, which is what changes the opening and gives the combo lift, texture, or contrast.`);
  }

  if (contrastHits.length) {
    whyParts.push(`The most useful contrast in this combo is ${contrastHits.slice(0, 3).join("; ")}, which gives the blend more dimension without completely changing its direction.`);
  }

  if (clashHits.length) {
    whyParts.push(`The main risk is ${clashHits.slice(0, 3).join("; ")}, which can make the blend feel disjointed or muddy if you overspray.`);
  }

  if (!clashHits.length && score >= 7) {
    whyParts.push("There are no major structural clashes in the pairing, so the blend should stay fairly coherent from opening through the drydown.");
  }

  if (score >= 8) {
    whyParts.push("This is the kind of combination where the base should carry the scent shape and the top should act like an accent rather than a second full fragrance competing for attention.");
  } else if (score >= 5) {
    whyParts.push("This is better treated like a controlled experiment: let the base do most of the work and use the top mainly to add color or brightness.");
  } else {
    whyParts.push("Even with a lighter ratio, the blend is likely to smell confused, so you will usually get a better result wearing one of them by itself.");
  }

  return { score, verdict, why: whyParts.join(" "), ratio, sprays };
}

function getLayeringSuggestions(collection, occasion, weather) {
  if (occasion === "Plane") {
    return [];
  }
  const pool = collection.filter((f) => (f.o || []).includes(occasion));
  const usablePool = pool.length ? pool : collection;
  const weightKey = occasion === "Special Event" ? "SpecialEvent" : occasion;
  const weights = getWeatherAdjustedWeights(RECOMMENDATION_WEIGHTS[weightKey] || {}, weather);

  const ranked = [...usablePool]
    .map((item) => ({ item, score: scoreFragrance(item, weights) }))
    .sort((a, b) => b.score - a.score)
    .map((entry) => entry.item);

  if (ranked.length < 2) {
    return [];
  }

  const suggestions = [];
  const seen = new Set();

  for (let i = 0; i < Math.min(ranked.length, 14); i += 1) {
    for (let j = 0; j < Math.min(ranked.length, 14); j += 1) {
      if (i === j) continue;
      const base = ranked[i];
      const top = ranked[j];
      const analysis = getLayeringAnalysis(base, top);
      if (analysis.score <= 4) continue;

      const key = base.n + "__" + top.n;
      const reverseKey = top.n + "__" + base.n;
      if (seen.has(key) || seen.has(reverseKey)) continue;

      seen.add(key);
      const candidateScore = analysis.score * 10 + scoreFragrance(base, weights) + scoreFragrance(top, weights);
      const weatherLine = getWeatherWhy(weather);
      const why = (analysis.why + " " + weatherLine).trim();

      suggestions.push({
        base,
        top,
        analysis,
        why,
        rank: candidateScore
      });
    }
  }

  return suggestions
    .sort((a, b) => b.rank - a.rank)
    .slice(0, 6);
}

function LayeringTab({ collection, theme }) {
  const [baseName, setBaseName] = useState(collection[0] ? collection[0].n : "");
  const [topName, setTopName] = useState(collection[1] ? collection[1].n : collection[0] ? collection[0].n : "");
  const [baseQuery, setBaseQuery] = useState("");
  const [topQuery, setTopQuery] = useState("");
  const [occasion, setOccasion] = useState("Office");
  const [environment, setEnvironment] = useState("Mixed");
  const { weather, locationMode, setLocationMode, manualCity, setManualCity } = useWeatherPreference();

  const sortedCollection = useMemo(() => {
    return [...collection].sort((a, b) => a.n.localeCompare(b.n));
  }, [collection]);

  const filteredBaseOptions = useMemo(() => {
    const q = baseQuery.trim().toLowerCase();
    if (!q) return sortedCollection;
    return sortedCollection.filter((f) => f.n.toLowerCase().includes(q) || (f.h || "").toLowerCase().includes(q));
  }, [sortedCollection, baseQuery]);

  const filteredTopOptions = useMemo(() => {
    const q = topQuery.trim().toLowerCase();
    if (!q) return sortedCollection;
    return sortedCollection.filter((f) => f.n.toLowerCase().includes(q) || (f.h || "").toLowerCase().includes(q));
  }, [sortedCollection, topQuery]);

  const base = useMemo(() => collection.find((f) => f.n === baseName), [collection, baseName]);
  const top = useMemo(() => collection.find((f) => f.n === topName), [collection, topName]);
  const analysis = useMemo(() => getLayeringAnalysis(base, top), [base, top]);
  const suggestions = useMemo(() => getLayeringSuggestions(collection, occasion, weather), [collection, occasion, weather]);

  return (
    <div className="grid gap-4">
      <Card theme={theme}>
        <div className="mb-4">
          <div className="text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>
            Layering advisor
          </div>
          <div className="text-sm" style={{ color: theme.muted }}>
            Choose a base fragrance and a top fragrance to see how the combo fits together.
          </div>
        </div>

        <div className="grid gap-4 md:grid-cols-2">
          <div>
            <label className="mb-2 block text-xs uppercase tracking-[0.18em]" style={{ color: theme.muted }}>
              Base fragrance
            </label>
            <input
              value={baseQuery}
              onChange={(e) => setBaseQuery(e.target.value)}
              placeholder="Search base fragrance"
              className="mb-2 w-full rounded-2xl border px-4 py-3 outline-none"
              style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }}
            />
            <select
              value={baseName}
              onChange={(e) => setBaseName(e.target.value)}
              className="w-full rounded-2xl border px-4 py-3 outline-none"
              style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }}
            >
              {filteredBaseOptions.map((f) => (
                <option key={f.n} value={f.n}>{f.n}</option>
              ))}
            </select>
          </div>

          <div>
            <label className="mb-2 block text-xs uppercase tracking-[0.18em]" style={{ color: theme.muted }}>
              Top fragrance
            </label>
            <input
              value={topQuery}
              onChange={(e) => setTopQuery(e.target.value)}
              placeholder="Search top fragrance"
              className="mb-2 w-full rounded-2xl border px-4 py-3 outline-none"
              style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }}
            />
            <select
              value={topName}
              onChange={(e) => setTopName(e.target.value)}
              className="w-full rounded-2xl border px-4 py-3 outline-none"
              style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }}
            >
              {filteredTopOptions.map((f) => (
                <option key={f.n} value={f.n}>{f.n}</option>
              ))}
            </select>
          </div>
        </div>
      </Card>

      <div className="grid gap-4 md:grid-cols-3">
        <Card theme={theme}>
          <div className="text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>Synergy score</div>
          <div className="mt-2 text-4xl font-semibold" style={{ color: theme.text }}>{analysis.score}/10</div>
          <div className="mt-2 text-sm" style={{ color: theme.muted }}>{analysis.verdict}</div>
        </Card>

        <Card theme={theme}>
          <div className="text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>Ratio</div>
          <div className="mt-2 text-lg font-semibold" style={{ color: theme.text }}>{analysis.ratio}</div>
          <div className="mt-2 text-sm" style={{ color: theme.muted }}>Use the heavier profile as the base when the combo leans rich.</div>
        </Card>

        <Card theme={theme}>
          <div className="text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>Spray plan</div>
          <div className="mt-2 text-sm" style={{ color: theme.text }}>{analysis.sprays}</div>
        </Card>
      </div>

      <Card theme={theme}>
        <div className="grid gap-4 md:grid-cols-2">
          <div className="rounded-2xl p-4" style={{ background: theme.panelAlt }}>
            <div className="text-xs uppercase tracking-[0.18em]" style={{ color: theme.accent }}>Base</div>
            <div className="mt-2 font-semibold" style={{ color: theme.text }}>{base ? base.n : "—"}</div>
            <div className="text-sm" style={{ color: theme.muted }}>{base ? base.h : ""}</div>
            <div className="mt-2 flex flex-wrap gap-2">
              {base && (base.d || []).map((note) => <Badge key={note} theme={theme}>{note}</Badge>)}
            </div>
            <div className="mt-3 text-sm" style={{ color: theme.muted }}>Inspired by / style: {base ? (base.i || "Original") : "—"}</div>
          </div>

          <div className="rounded-2xl p-4" style={{ background: theme.panelAlt }}>
            <div className="text-xs uppercase tracking-[0.18em]" style={{ color: theme.accent }}>Top</div>
            <div className="mt-2 font-semibold" style={{ color: theme.text }}>{top ? top.n : "—"}</div>
            <div className="text-sm" style={{ color: theme.muted }}>{top ? top.h : ""}</div>
            <div className="mt-2 flex flex-wrap gap-2">
              {top && (top.d || []).map((note) => <Badge key={note} theme={theme}>{note}</Badge>)}
            </div>
            <div className="mt-3 text-sm" style={{ color: theme.muted }}>Inspired by / style: {top ? (top.i || "Original") : "—"}</div>
          </div>
        </div>

        <div className="mt-4 rounded-2xl p-4" style={{ background: theme.panelAlt }}>
          <div className="text-xs uppercase tracking-[0.18em]" style={{ color: theme.accent }}>Why this works</div>
          <div className="mt-2 text-sm" style={{ color: theme.text }}>{analysis.why}</div>
        </div>
      </Card>

      <Card theme={theme}>
        <div className="mb-4 flex flex-col gap-3 md:flex-row md:items-end md:justify-between">
          <div>
            <div className="text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>
              Suggestion box
            </div>
            <div className="text-sm" style={{ color: theme.muted }}>
              Weather- and occasion-based layering suggestions with base, top, spray count, and why they work.
            </div>
          </div>
          <div className="grid gap-3 md:grid-cols-2 md:min-w-[520px]">
            <div>
              <label className="mb-2 block text-xs uppercase tracking-[0.18em]" style={{ color: theme.muted }}>
                Occasion
              </label>
              <select
                value={occasion}
                onChange={(e) => setOccasion(e.target.value)}
                className="w-full rounded-2xl border px-4 py-3 outline-none"
                style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }}
              >
                {QUICK_OCCASIONS.map((item) => (
                  <option key={item} value={item}>{item}</option>
                ))}
              </select>
            </div>
            <div>
              <label className="mb-2 block text-xs uppercase tracking-[0.18em]" style={{ color: theme.muted }}>
                Environment
              </label>
              <select
                value={environment}
                onChange={(e) => setEnvironment(e.target.value)}
                className="w-full rounded-2xl border px-4 py-3 outline-none"
                style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }}
              >
                {ENVIRONMENTS.map((item) => (
                  <option key={item} value={item}>{item}</option>
                ))}
              </select>
            </div>
          </div>
        </div>

        <div className="mb-4 rounded-2xl border px-4 py-3" style={{ background: theme.panelAlt, borderColor: theme.border }}>
          <div className="text-xs uppercase tracking-[0.16em]" style={{ color: theme.muted }}>Weather</div>
          <div className="mt-1 text-sm" style={{ color: theme.text }}>
            {weather.loading
              ? "Loading weather..."
              : weather.error
              ? weather.error
              : `${weather.city} • ${weather.temp !== null ? Math.round(weather.temp) : "--"}°F • ${getWeatherLabel(weather.code)} • ${environment}`}
          </div>
        </div>

        {suggestions.length ? (
          <div className="grid gap-4 md:grid-cols-2 xl:grid-cols-3">
            {suggestions.map((suggestion, index) => (
              <div key={suggestion.base.n + suggestion.top.n} className="rounded-2xl p-4" style={{ background: theme.panelAlt }}>
                <div className="text-xs uppercase tracking-[0.18em]" style={{ color: theme.accent }}>
                  Suggestion {index + 1}
                </div>
                <div className="mt-3 text-sm" style={{ color: theme.muted }}>Base</div>
                <div className="font-semibold" style={{ color: theme.text }}>{suggestion.base.n}</div>
                <div className="text-sm" style={{ color: theme.muted }}>{suggestion.base.h}</div>
                <div className="mt-3 text-sm" style={{ color: theme.muted }}>Top</div>
                <div className="font-semibold" style={{ color: theme.text }}>{suggestion.top.n}</div>
                <div className="text-sm" style={{ color: theme.muted }}>{suggestion.top.h}</div>
                <div className="mt-3 text-sm" style={{ color: theme.muted }}>Score</div>
                <div className="font-semibold" style={{ color: theme.text }}>{suggestion.analysis.score}/10 · {suggestion.analysis.verdict}</div>
                <div className="mt-3 text-sm" style={{ color: theme.muted }}>Ratio</div>
                <div className="text-sm" style={{ color: theme.text }}>{suggestion.analysis.ratio}</div>
                <div className="mt-3 text-sm" style={{ color: theme.muted }}>Sprays</div>
                <div className="text-sm" style={{ color: theme.text }}>{suggestion.analysis.sprays}</div>
                <div className="mt-3 text-sm" style={{ color: theme.muted }}>Why</div>
                <div className="text-sm" style={{ color: theme.text }}>{suggestion.why}</div>
              </div>
            ))}
          </div>
        ) : (
          <div className="rounded-2xl p-4" style={{ background: theme.panelAlt, color: theme.muted }}>
            No strong layering suggestion available for this weather and occasion.
          </div>
        )}
      </Card>
    </div>
  );
}

function WearTodayTab({ collection, theme }) {
  const [selected, setSelected] = useState(collection[0] ? collection[0].n : "");
  const [sprays, setSprays] = useState("3");
  const [notes, setNotes] = useState("");
  const [wears, setWears] = useState([]);
  const [occasion, setOccasion] = useState("Office");
  const [environment, setEnvironment] = useState("Mixed");
  const { weather } = useWeatherPreference();
  const today = new Date().toISOString().slice(0, 10);

  useEffect(() => {
    try {
      const saved = localStorage.getItem("vault_wear_log");
      if (saved) setWears(JSON.parse(saved));
    } catch (e) {}
  }, []);

  useEffect(() => {
    try {
      localStorage.setItem("vault_wear_log", JSON.stringify(wears));
    } catch (e) {}
  }, [wears]);

  const sortedCollection = useMemo(() => {
    return [...collection].sort((a, b) => a.n.localeCompare(b.n));
  }, [collection]);

  const selectedFragrance = useMemo(() => {
    return collection.find((f) => f.n === selected) || null;
  }, [collection, selected]);

  const manualSpraySuggestion = useMemo(() => {
    return selectedFragrance ? getSprayAdvice(selectedFragrance, occasion) : "Select a fragrance to see spray guidance.";
  }, [selectedFragrance, occasion]);

  const recommendationPool = useMemo(() => {
    const byOccasion = collection.filter((f) => (f.o || []).includes(occasion));
    let pool = byOccasion.length ? byOccasion : collection;
    if (occasion === "Plane") {
      const planePool = pool.filter((f) => isPlaneSafeFragrance(f));
      pool = planePool.length ? planePool : pool;
    }
    return pool;
  }, [collection, occasion]);

  const generatedPicks = useMemo(() => {
    const weightKey = occasion === "Special Event" ? "SpecialEvent" : occasion;
    const weights = getWeatherAdjustedWeights(RECOMMENDATION_WEIGHTS[weightKey] || {}, weather);
    let picks = pickTop(recommendationPool, weights, 8, weather, environment);
    if (occasion === "Plane") {
      picks = picks.filter((f) => isPlaneSafeFragrance(f));
    }
    return picks.slice(0, 5);
  }, [recommendationPool, occasion, weather, environment]);

  const applyGeneratedPick = (pick) => {
    if (!pick) return;
    setSelected(pick.n);
    const sprayText = getSprayAdvice(pick, occasion);
    const firstNum = (sprayText.match(/[0-9]+/) || ["3"])[0];
    setSprays(firstNum);
    if (!notes) {
      const weatherText = weather && weather.temp !== null ? " • " + Math.round(weather.temp) + "°F " + getWeatherLabel(weather.code) : "";
      setNotes("Generated pick for " + occasion + weatherText + " • " + environment + ".");
    }
  };

  const addWear = () => {
    if (!selected) return;
    const entry = {
      id: Date.now(),
      date: today,
      fragrance: selected,
      house: selectedFragrance ? selectedFragrance.h : "",
      sprays: sprays || "",
      notes: notes.trim(),
      occasion
    };
    setWears((prev) => [entry, ...prev]);
    setNotes("");
  };

  const deleteWear = (id) => {
    setWears((prev) => prev.filter((item) => item.id !== id));
  };

  const todayCount = wears.filter((w) => w.date === today).length;

  return (
    <div className="grid gap-4">
      <Card theme={theme}>
        <div className="mb-4">
          <div className="text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>
            Wear today
          </div>
          <div className="text-sm" style={{ color: theme.muted }}>
            Generate a pick from weather, occasion, and environment, then log what you wore.
          </div>
        </div>

        <div className="grid gap-4 md:grid-cols-2">
          <div>
            <label className="mb-2 block text-xs uppercase tracking-[0.18em]" style={{ color: theme.muted }}>
              Occasion
            </label>
            <select
              value={occasion}
              onChange={(e) => setOccasion(e.target.value)}
              className="w-full rounded-2xl border px-4 py-3 outline-none"
              style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }}
            >
              {QUICK_OCCASIONS.map((item) => (
                <option key={item} value={item}>{item}</option>
              ))}
            </select>
          </div>

          <div>
            <label className="mb-2 block text-xs uppercase tracking-[0.18em]" style={{ color: theme.muted }}>
              Environment
            </label>
            <select
              value={environment}
              onChange={(e) => setEnvironment(e.target.value)}
              className="w-full rounded-2xl border px-4 py-3 outline-none"
              style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }}
            >
              {ENVIRONMENTS.map((item) => (
                <option key={item} value={item}>{item}</option>
              ))}
            </select>
          </div>

          <div>
            <label className="mb-2 block text-xs uppercase tracking-[0.18em]" style={{ color: theme.muted }}>
              Top 5 picks
            </label>
            <div className="space-y-2">
              {generatedPicks.length ? generatedPicks.map((pick, index) => (
                <button
                  key={pick.n}
                  onClick={() => applyGeneratedPick(pick)}
                  className="w-full rounded-2xl border px-4 py-3 text-left"
                  style={{ background: theme.panelAlt, borderColor: theme.border }}
                >
                  <div className="flex items-start justify-between gap-3">
                    <div>
                      <div className="text-xs uppercase tracking-[0.16em]" style={{ color: theme.accent }}>Pick {index + 1}</div>
                      <div className="font-semibold" style={{ color: theme.text }}>{pick.n}</div>
                      <div className="text-sm" style={{ color: theme.muted }}>{pick.h}</div>
                      <div className="mt-2 text-sm" style={{ color: theme.text }}>
                        Why: {getWhyWear(pick, occasion, weather)}
                      </div>
                      <div className="mt-1 text-sm" style={{ color: theme.text }}>
                        Sprays: {getSprayAdvice(pick, occasion)}
                      </div>
                    </div>
                    <div className="text-xs" style={{ color: theme.muted }}>Use</div>
                  </div>
                </button>
              )) : (
                <div className="rounded-2xl border px-4 py-3" style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.muted }}>
                  No picks available.
                </div>
              )}
            </div>
          </div>

          <div>
            <label className="mb-2 block text-xs uppercase tracking-[0.18em]" style={{ color: theme.muted }}>
              Fragrance
            </label>
            <select
              value={selected}
              onChange={(e) => setSelected(e.target.value)}
              className="w-full rounded-2xl border px-4 py-3 outline-none"
              style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }}
            >
              {sortedCollection.map((f) => (
                <option key={f.n} value={f.n}>{f.n}</option>
              ))}
            </select>
          </div>

          <div>
            <label className="mb-2 block text-xs uppercase tracking-[0.18em]" style={{ color: theme.muted }}>
              Sprays
            </label>
            <input
              value={sprays}
              onChange={(e) => setSprays(e.target.value)}
              placeholder="How many sprays"
              className="w-full rounded-2xl border px-4 py-3 outline-none"
              style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }}
            />
          </div>
        </div>

        <div className="mt-4 rounded-2xl border px-4 py-3" style={{ background: theme.panelAlt, borderColor: theme.border }}>
          <div className="text-xs uppercase tracking-[0.16em]" style={{ color: theme.accent }}>
            Manual pick spray suggestion
          </div>
          <div className="mt-2 text-sm" style={{ color: theme.text }}>
            {manualSpraySuggestion}
          </div>
          <div className="mt-1 text-sm" style={{ color: theme.muted }}>
            This updates when you manually change the fragrance or occasion.
          </div>
        </div>

        <div className="mt-4 flex flex-wrap items-center justify-between gap-3">
          <div style={{ color: theme.muted }}>
            {weather.loading ? "Loading weather for recommendation..." : weather.error ? weather.error : `${weather.city} • ${weather.temp !== null ? Math.round(weather.temp) : "--"}°F • ${getWeatherLabel(weather.code)} • ${environment}`}
          </div>
          <div className="text-sm" style={{ color: theme.muted }}>
            Tap any pick to load it into the fragrance field below.
          </div>
        </div>

        <div className="mt-4">
          <label className="mb-2 block text-xs uppercase tracking-[0.18em]" style={{ color: theme.muted }}>
            Notes
          </label>
          <textarea
            value={notes}
            onChange={(e) => setNotes(e.target.value)}
            placeholder="Optional notes: weather, occasion, performance, compliments, mood"
            className="min-h-[120px] w-full rounded-2xl border px-4 py-3 outline-none"
            style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }}
          />
        </div>

        <div className="mt-4 flex flex-wrap items-center justify-between gap-3">
          <div style={{ color: theme.muted }}>
            Today: <span style={{ color: theme.text }}>{today}</span> · Logged wears: <span style={{ color: theme.text }}>{todayCount}</span>
          </div>
          <button
            onClick={addWear}
            className="rounded-2xl border px-4 py-3 text-sm"
            style={{ borderColor: theme.accent, background: theme.accentSoft, color: theme.text }}
          >
            Save wear
          </button>
        </div>
      </Card>

      <Card theme={theme}>
        <div className="mb-4 flex items-center justify-between gap-3">
          <div>
            <div className="text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>
              Wear history
            </div>
            <div className="text-sm" style={{ color: theme.muted }}>
              Most recent wear entries saved in the app.
            </div>
          </div>
          <div style={{ color: theme.muted }}>{wears.length} entries</div>
        </div>

        <div className="space-y-3">
          {wears.length === 0 ? (
            <div className="rounded-2xl p-4" style={{ background: theme.panelAlt, color: theme.muted }}>
              No wears logged yet.
            </div>
          ) : (
            wears.map((entry) => (
              <div key={entry.id} className="rounded-2xl p-4" style={{ background: theme.panelAlt }}>
                <div className="flex flex-wrap items-start justify-between gap-3">
                  <div>
                    <div className="font-semibold" style={{ color: theme.text }}>{entry.fragrance}</div>
                    <div className="text-sm" style={{ color: theme.muted }}>{entry.house}</div>
                    <div className="mt-2 text-sm" style={{ color: theme.muted }}>
                      Date: {entry.date} · Occasion: {entry.occasion || "—"} · Sprays: {entry.sprays || "—"}
                    </div>
                    {entry.notes ? <div className="mt-2 text-sm" style={{ color: theme.text }}>{entry.notes}</div> : null}
                  </div>
                  <button
                    onClick={() => deleteWear(entry.id)}
                    className="rounded-xl border px-3 py-2 text-sm"
                    style={{ borderColor: theme.border, color: theme.muted }}
                  >
                    Delete
                  </button>
                </div>
              </div>
            ))
          )}
        </div>
      </Card>
    </div>
  );
}

function getCandidateOverlap(candidate, existing) {
  const candidateDNA = candidate.d || [];
  const existingDNA = existing.d || [];
  const sharedDNA = candidateDNA.filter((note) => existingDNA.includes(note));
  const candidateOccasions = candidate.o || [];
  const existingOccasions = existing.o || [];
  const sharedOccasions = candidateOccasions.filter((item) => existingOccasions.includes(item));
  let score = sharedDNA.length * 2 + sharedOccasions.length;
  if (candidate.i && existing.i && candidate.i.toLowerCase() === existing.i.toLowerCase()) score += 4;
  if (candidate.s && existing.s && candidate.s === existing.s) score += 1;
  return { score, sharedDNA, sharedOccasions };
}

function classifyBuyDecision(bestMatch, candidate) {
  const dnaCount = (candidate.d || []).length;
  if (!bestMatch) {
    return {
      verdict: "Interesting but unclear",
      reason: "There is not enough comparable metadata to judge this one confidently against your collection."
    };
  }

  if (bestMatch.overlap.score >= 9) {
    return {
      verdict: "Good but redundant",
      reason: `It overlaps heavily with ${bestMatch.item.n} and does not appear to open a major new lane.`
    };
  }

  if (bestMatch.overlap.score >= 6) {
    return {
      verdict: "Interesting but unnecessary",
      reason: `It has some appeal, but it still sits fairly close to ${bestMatch.item.n} in use and profile.`
    };
  }

  if (dnaCount <= 1) {
    return {
      verdict: "Interesting but unclear",
      reason: "The input is too thin to call this a real gap-filler yet."
    };
  }

  return {
    verdict: "Worth considering",
    reason: "It looks different enough from your closest match to potentially fill a new role in the collection."
  };
}

function getSeasonBucketLabel(season) {
  const s = (season || "").toLowerCase();
  if (s.includes("fall") || s.includes("winter")) return "Fall/Winter";
  if (s.includes("spring") || s.includes("summer")) return "Spring/Summer";
  if (s.includes("all season")) return "All Season";
  return "Flexible";
}

function scoreDisplayFragrance(f, targetSeason) {
  const dna = f.d || [];
  const season = (f.s || "").toLowerCase();
  let score = f.r || 0;

  const boosts = {
    Spring: ["spring", "spring/fall", "spring/summer", "year-round", "all season"],
    Summer: ["summer", "spring/summer", "year-round", "all season"],
    Fall: ["fall", "spring/fall", "fall/winter", "year-round", "all season"],
    Winter: ["winter", "fall/winter", "year-round", "all season"]
  };

  const penalties = {
    Spring: ["fall/winter"],
    Summer: ["fall/winter"],
    Fall: ["spring/summer"],
    Winter: ["spring/summer"]
  };

  (boosts[targetSeason] || []).forEach((tag) => {
    if (season.includes(tag)) score += tag === "year-round" || tag === "all season" ? 2 : 4;
  });

  (penalties[targetSeason] || []).forEach((tag) => {
    if (season.includes(tag)) score -= 3;
  });

  if (targetSeason === "Spring") {
    if (dna.includes("green") || dna.includes("fresh") || dna.includes("citrus") || dna.includes("floral") || dna.includes("clean") || dna.includes("vetiver")) score += 3;
    if (dna.includes("oud") || dna.includes("dark") || dna.includes("gourmand") || dna.includes("tobacco")) score -= 2;
  }

  if (targetSeason === "Summer") {
    if (dna.includes("fresh") || dna.includes("aquatic") || dna.includes("citrus") || dna.includes("blue") || dna.includes("clean") || dna.includes("marine")) score += 3;
    if (dna.includes("amber") || dna.includes("oud") || dna.includes("tobacco") || dna.includes("dark") || dna.includes("gourmand")) score -= 3;
  }

  if (targetSeason === "Fall") {
    if (dna.includes("woody") || dna.includes("amber") || dna.includes("spicy") || dna.includes("vetiver") || dna.includes("fougere")) score += 3;
    if (dna.includes("marine") || dna.includes("aquatic")) score -= 1;
  }

  if (targetSeason === "Winter") {
    if (dna.includes("amber") || dna.includes("oud") || dna.includes("tobacco") || dna.includes("dark") || dna.includes("gourmand") || dna.includes("oriental")) score += 3;
    if (dna.includes("aquatic") || dna.includes("marine") || dna.includes("citrus") || dna.includes("fresh")) score -= 2;
  }

  return score;
}

function getDisplaySections(collection, targetSeason) {
  const ranked = [...collection]
    .map((f) => ({ item: f, score: scoreDisplayFragrance(f, targetSeason) }))
    .sort((a, b) => b.score - a.score)
    .map((entry) => entry.item);

  const anchors = [];
  const daily = [];
  const statement = [];
  const reserve = [];
  const used = new Set();

  const addUnique = (bucket, fragrance) => {
    if (!fragrance || used.has(fragrance.n)) return;
    used.add(fragrance.n);
    bucket.push(fragrance);
  };

  ranked.forEach((f) => {
    const dna = f.d || [];
    if (anchors.length < 8 && f.r >= 4) {
      addUnique(anchors, f);
      return;
    }
    if (daily.length < 12 && (dna.includes("fresh") || dna.includes("woody") || dna.includes("clean") || dna.includes("blue") || dna.includes("green") || dna.includes("vetiver"))) {
      addUnique(daily, f);
      return;
    }
    if (statement.length < 10 && (dna.includes("amber") || dna.includes("oud") || dna.includes("tobacco") || dna.includes("dark") || dna.includes("gourmand") || dna.includes("oriental") || f.o?.includes("Special Event") || f.o?.includes("Evening"))) {
      addUnique(statement, f);
      return;
    }
    addUnique(reserve, f);
  });

  const display = [...anchors, ...daily, ...statement];
  while (display.length < 30 && reserve.length) {
    addUnique(display, reserve.shift());
  }

  return {
    display: display.slice(0, 30),
    anchors: anchors.slice(0, 8),
    daily: daily.slice(0, 12),
    statement: statement.slice(0, 10)
  };
}

function DisplayTab({ collection, theme }) {
  const [seasonMode, setSeasonMode] = useState("Auto");

  const autoSeason = useMemo(() => {
    const month = new Date().getMonth() + 1;
    if (month >= 3 && month <= 5) return "Spring";
    if (month >= 6 && month <= 8) return "Summer";
    if (month >= 9 && month <= 11) return "Fall";
    return "Winter";
  }, []);

  const activeSeason = seasonMode === "Auto" ? autoSeason : seasonMode;

  const activeSections = useMemo(() => getDisplaySections(collection, activeSeason), [collection, activeSeason]);
  const springSections = useMemo(() => getDisplaySections(collection, "Spring"), [collection]);
  const summerSections = useMemo(() => getDisplaySections(collection, "Summer"), [collection]);
  const fallSections = useMemo(() => getDisplaySections(collection, "Fall"), [collection]);
  const winterSections = useMemo(() => getDisplaySections(collection, "Winter"), [collection]);

  const seasonalPanels = [
    { key: "Spring", title: "Spring Display", sections: springSections },
    { key: "Summer", title: "Summer Display", sections: summerSections },
    { key: "Fall", title: "Fall Display", sections: fallSections },
    { key: "Winter", title: "Winter Display", sections: winterSections }
  ];

  return (
    <div className="grid gap-4">
      <Card theme={theme}>
        <div className="mb-4 flex flex-col gap-3 md:flex-row md:items-end md:justify-between">
          <div>
            <div className="text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>
              Display planner
            </div>
            <div className="text-sm" style={{ color: theme.muted }}>
              Keep Auto for the current season, but also browse separate Spring, Summer, Fall, and Winter 30-bottle displays.
            </div>
          </div>
          <div className="w-full md:w-64">
            <label className="mb-2 block text-xs uppercase tracking-[0.18em]" style={{ color: theme.muted }}>
              Season mode
            </label>
            <select
              value={seasonMode}
              onChange={(e) => setSeasonMode(e.target.value)}
              className="w-full rounded-2xl border px-4 py-3 outline-none"
              style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }}
            >
              <option value="Auto">Auto</option>
              <option value="Spring">Spring</option>
              <option value="Summer">Summer</option>
              <option value="Fall">Fall</option>
              <option value="Winter">Winter</option>
            </select>
          </div>
        </div>

        <div className="grid gap-4 md:grid-cols-5">
          <div className="rounded-2xl p-4" style={{ background: theme.panelAlt }}>
            <div className="text-xs uppercase tracking-[0.18em]" style={{ color: theme.accent }}>Active season</div>
            <div className="mt-2 text-lg font-semibold" style={{ color: theme.text }}>{activeSeason}</div>
          </div>
          <div className="rounded-2xl p-4" style={{ background: theme.panelAlt }}>
            <div className="text-xs uppercase tracking-[0.18em]" style={{ color: theme.accent }}>Spring</div>
            <div className="mt-2 text-lg font-semibold" style={{ color: theme.text }}>{springSections.display.length}</div>
          </div>
          <div className="rounded-2xl p-4" style={{ background: theme.panelAlt }}>
            <div className="text-xs uppercase tracking-[0.18em]" style={{ color: theme.accent }}>Summer</div>
            <div className="mt-2 text-lg font-semibold" style={{ color: theme.text }}>{summerSections.display.length}</div>
          </div>
          <div className="rounded-2xl p-4" style={{ background: theme.panelAlt }}>
            <div className="text-xs uppercase tracking-[0.18em]" style={{ color: theme.accent }}>Fall</div>
            <div className="mt-2 text-lg font-semibold" style={{ color: theme.text }}>{fallSections.display.length}</div>
          </div>
          <div className="rounded-2xl p-4" style={{ background: theme.panelAlt }}>
            <div className="text-xs uppercase tracking-[0.18em]" style={{ color: theme.accent }}>Winter</div>
            <div className="mt-2 text-lg font-semibold" style={{ color: theme.text }}>{winterSections.display.length}</div>
          </div>
        </div>
      </Card>

      <Card theme={theme}>
        <div className="mb-4">
          <div className="text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>
            Active display summary
          </div>
          <div className="text-sm" style={{ color: theme.muted }}>
            This is the display currently selected by your Season mode.
          </div>
        </div>
        <div className="grid gap-4 md:grid-cols-3">
          <div className="rounded-2xl p-4" style={{ background: theme.panelAlt }}>
            <div className="text-xs uppercase tracking-[0.18em]" style={{ color: theme.accent }}>Anchors</div>
            <div className="mt-2 text-lg font-semibold" style={{ color: theme.text }}>{activeSections.anchors.length}</div>
          </div>
          <div className="rounded-2xl p-4" style={{ background: theme.panelAlt }}>
            <div className="text-xs uppercase tracking-[0.18em]" style={{ color: theme.accent }}>Daily Drivers</div>
            <div className="mt-2 text-lg font-semibold" style={{ color: theme.text }}>{activeSections.daily.length}</div>
          </div>
          <div className="rounded-2xl p-4" style={{ background: theme.panelAlt }}>
            <div className="text-xs uppercase tracking-[0.18em]" style={{ color: theme.accent }}>Statement Pieces</div>
            <div className="mt-2 text-lg font-semibold" style={{ color: theme.text }}>{activeSections.statement.length}</div>
          </div>
        </div>
      </Card>

      {seasonalPanels.map((panel) => (
        <div key={panel.key} className="grid gap-4">
          {[{ title: `${panel.title} · Anchors`, items: panel.sections.anchors, note: "Your strongest seasonal core bottles." }, { title: `${panel.title} · Daily Drivers`, items: panel.sections.daily, note: "Easy reaches for regular wear." }, { title: `${panel.title} · Statement Pieces`, items: panel.sections.statement, note: "Distinctive bottles that round out the shelf." }].map((section) => (
            <Card theme={theme} key={section.title}>
              <div className="mb-4">
                <div className="text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>{section.title}</div>
                <div className="text-sm" style={{ color: theme.muted }}>{section.note}</div>
              </div>
              <div className="grid gap-3 md:grid-cols-2 xl:grid-cols-3">
                {section.items.map((f) => (
                  <div key={panel.key + section.title + f.n} className="rounded-2xl p-4" style={{ background: theme.panelAlt }}>
                    <div className="font-semibold" style={{ color: theme.text }}>{f.n}</div>
                    <div className="text-sm" style={{ color: theme.muted }}>{f.h}</div>
                    <div className="mt-2 flex flex-wrap gap-2">
                      {(f.d || []).map((note) => (
                        <Badge key={note} theme={theme}>{note}</Badge>
                      ))}
                    </div>
                    <div className="mt-3 text-sm" style={{ color: theme.muted }}>Season: {f.s}</div>
                    <div className="text-sm" style={{ color: theme.muted }}>Inspired by / style: {f.i || "Original"}</div>
                  </div>
                ))}
              </div>
            </Card>
          ))}
        </div>
      ))}
    </div>
  );
}

function BuyOrSkipTab({ collection, theme }) {
  const [name, setName] = useState("");
  const [house, setHouse] = useState("");
  const [inspiration, setInspiration] = useState("");
  const [dnaText, setDnaText] = useState("");
  const [occasionText, setOccasionText] = useState("");
  const [season, setSeason] = useState("All Season");

  const candidate = useMemo(() => {
    const dna = dnaText
      .split(",")
      .map((s) => s.trim())
      .filter(Boolean);
    const occasions = occasionText
      .split(",")
      .map((s) => s.trim())
      .filter(Boolean);
    return {
      n: name.trim(),
      h: house.trim(),
      i: inspiration.trim(),
      d: dna,
      o: occasions,
      s: season,
      r: 3
    };
  }, [name, house, inspiration, dnaText, occasionText, season]);

  const rankedMatches = useMemo(() => {
    if (!candidate.n && !candidate.i && !(candidate.d || []).length) return [];
    return collection
      .map((item) => ({ item, overlap: getCandidateOverlap(candidate, item) }))
      .sort((a, b) => b.overlap.score - a.overlap.score)
      .slice(0, 5);
  }, [candidate, collection]);

  const bestMatch = rankedMatches.length ? rankedMatches[0] : null;
  const decision = useMemo(() => classifyBuyDecision(bestMatch, candidate), [bestMatch, candidate]);

  return (
    <div className="grid gap-4">
      <Card theme={theme}>
        <div className="mb-4">
          <div className="text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>
            Buy or Skip
          </div>
          <div className="text-sm" style={{ color: theme.muted }}>
            Enter a fragrance you are considering and compare it against your collection to see whether it fills a gap or feels redundant.
          </div>
        </div>

        <div className="grid gap-4 md:grid-cols-2">
          <div>
            <label className="mb-2 block text-xs uppercase tracking-[0.18em]" style={{ color: theme.muted }}>Fragrance name</label>
            <input value={name} onChange={(e) => setName(e.target.value)} className="w-full rounded-2xl border px-4 py-3 outline-none" style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }} placeholder="Example: Torino 21" />
          </div>
          <div>
            <label className="mb-2 block text-xs uppercase tracking-[0.18em]" style={{ color: theme.muted }}>House</label>
            <input value={house} onChange={(e) => setHouse(e.target.value)} className="w-full rounded-2xl border px-4 py-3 outline-none" style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }} placeholder="Example: Xerjoff" />
          </div>
          <div>
            <label className="mb-2 block text-xs uppercase tracking-[0.18em]" style={{ color: theme.muted }}>Inspired by / style</label>
            <input value={inspiration} onChange={(e) => setInspiration(e.target.value)} className="w-full rounded-2xl border px-4 py-3 outline-none" style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }} placeholder="Example: Bleu de Chanel" />
          </div>
          <div>
            <label className="mb-2 block text-xs uppercase tracking-[0.18em]" style={{ color: theme.muted }}>Season</label>
            <select value={season} onChange={(e) => setSeason(e.target.value)} className="w-full rounded-2xl border px-4 py-3 outline-none" style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }}>
              {[
                "All Season",
                "Spring/Summer",
                "Spring/Fall",
                "Fall/Winter",
                "Summer",
                "Spring",
                "Fall"
              ].map((item) => <option key={item} value={item}>{item}</option>)}
            </select>
          </div>
        </div>

        <div className="mt-4 grid gap-4 md:grid-cols-2">
          <div>
            <label className="mb-2 block text-xs uppercase tracking-[0.18em]" style={{ color: theme.muted }}>DNA tags</label>
            <input value={dnaText} onChange={(e) => setDnaText(e.target.value)} className="w-full rounded-2xl border px-4 py-3 outline-none" style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }} placeholder="fresh, woody, citrus, aquatic" />
          </div>
          <div>
            <label className="mb-2 block text-xs uppercase tracking-[0.18em]" style={{ color: theme.muted }}>Occasions</label>
            <input value={occasionText} onChange={(e) => setOccasionText(e.target.value)} className="w-full rounded-2xl border px-4 py-3 outline-none" style={{ background: theme.panelAlt, borderColor: theme.border, color: theme.text }} placeholder="Office, Casual, Date" />
          </div>
        </div>
      </Card>

      <div className="grid gap-4 md:grid-cols-3">
        <Card theme={theme}>
          <div className="text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>Verdict</div>
          <div className="mt-2 text-xl font-semibold" style={{ color: theme.text }}>{decision.verdict}</div>
          <div className="mt-2 text-sm" style={{ color: theme.text }}>{decision.reason}</div>
        </Card>

        <Card theme={theme}>
          <div className="text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>Closest match</div>
          <div className="mt-2 text-lg font-semibold" style={{ color: theme.text }}>{bestMatch ? bestMatch.item.n : "—"}</div>
          <div className="text-sm" style={{ color: theme.muted }}>{bestMatch ? bestMatch.item.h : ""}</div>
          <div className="mt-2 text-sm" style={{ color: theme.text }}>Overlap score: {bestMatch ? bestMatch.overlap.score : "—"}</div>
        </Card>

        <Card theme={theme}>
          <div className="text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>Why</div>
          <div className="mt-2 text-sm" style={{ color: theme.text }}>
            {bestMatch
              ? `Shared DNA: ${bestMatch.overlap.sharedDNA.join(", ") || "none"}. Shared occasions: ${bestMatch.overlap.sharedOccasions.join(", ") || "none"}.`
              : "Add candidate details to compare it against your collection."}
          </div>
        </Card>
      </div>

      <Card theme={theme}>
        <div className="mb-4">
          <div className="text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>Top overlaps</div>
          <div className="text-sm" style={{ color: theme.muted }}>These are the bottles in your collection that feel closest to the candidate.</div>
        </div>
        <div className="space-y-3">
          {rankedMatches.length ? rankedMatches.map(({ item, overlap }) => (
            <div key={item.n} className="rounded-2xl p-4" style={{ background: theme.panelAlt }}>
              <div className="font-semibold" style={{ color: theme.text }}>{item.n}</div>
              <div className="text-sm" style={{ color: theme.muted }}>{item.h}</div>
              <div className="mt-2 text-sm" style={{ color: theme.text }}>Overlap score: {overlap.score}</div>
              <div className="text-sm" style={{ color: theme.muted }}>DNA overlap: {overlap.sharedDNA.join(", ") || "none"}</div>
              <div className="text-sm" style={{ color: theme.muted }}>Occasion overlap: {overlap.sharedOccasions.join(", ") || "none"}</div>
              <div className="text-sm" style={{ color: theme.muted }}>Inspired by / style: {item.i || "Original"}</div>
            </div>
          )) : (
            <div className="rounded-2xl p-4" style={{ background: theme.panelAlt, color: theme.muted }}>
              Add a fragrance name plus at least some DNA or inspiration info to run the comparison.
            </div>
          )}
        </div>
      </Card>
    </div>
  );
}

function AnalyticsTab({ collection, theme }) {
  const byHouse = useMemo(() => {
    const map = new Map();
    collection.forEach((f) => map.set(f.h, (map.get(f.h) || 0) + 1));
    return [...map.entries()].sort((a, b) => b[1] - a[1]);
  }, [collection]);

  const bySeason = useMemo(() => {
    const map = new Map();
    collection.forEach((f) => map.set(f.s, (map.get(f.s) || 0) + 1));
    return [...map.entries()].sort((a, b) => b[1] - a[1]);
  }, [collection]);

  return (
    <div className="grid gap-4 md:grid-cols-2">
      <Card theme={theme}>
        <div className="mb-3 text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>
          By house
        </div>
        <div className="space-y-2">
          {byHouse.map(([house, count]) => (
            <div key={house} className="flex items-center justify-between rounded-2xl px-3 py-2" style={{ background: theme.panelAlt }}>
              <span style={{ color: theme.text }}>{house}</span>
              <span style={{ color: theme.muted }}>{count}</span>
            </div>
          ))}
        </div>
      </Card>

      <Card theme={theme}>
        <div className="mb-3 text-xs uppercase tracking-[0.2em]" style={{ color: theme.accent }}>
          By season
        </div>
        <div className="space-y-2">
          {bySeason.map(([season, count]) => (
            <div key={season} className="flex items-center justify-between rounded-2xl px-3 py-2" style={{ background: theme.panelAlt }}>
              <span style={{ color: theme.text }}>{season}</span>
              <span style={{ color: theme.muted }}>{count}</span>
            </div>
          ))}
        </div>
      </Card>
    </div>
  );
}

function SafeApp() {
  const [tab, setTab] = useState("Home");
  const [query, setQuery] = useState("");
  const [mode, setMode] = useState("dark");
  const collection = Array.isArray(COLL) ? COLL : [];
  const theme = mode === "dark" ? darkTheme : lightTheme;

  return (
    <div className="min-h-screen p-6 md:p-10" style={{ background: theme.bg, color: theme.text }}>
      <div className="mx-auto max-w-7xl space-y-6">
        <div className="flex flex-col gap-4 md:flex-row md:items-end md:justify-between">
          <div>
            <div className="text-xs uppercase tracking-[0.3em]" style={{ color: theme.accent }}>
              The Vault
            </div>
            <h1 className="text-4xl font-semibold">AJ’s Fragrance Collection</h1>
            <p style={{ color: theme.muted }}>
              {collection.length} fragrances loaded from your PDF-based collection.
            </p>
          </div>
          <div className="flex flex-wrap items-center gap-2">
            <ThemeToggle mode={mode} setMode={setMode} theme={theme} />
            {TABS.map((name) => {
              const active = tab === name;
              return (
                <button
                  key={name}
                  onClick={() => setTab(name)}
                  className="rounded-2xl border px-4 py-2 text-sm"
                  style={{
                    borderColor: active ? theme.accent : theme.border,
                    background: active ? theme.accentSoft : theme.panel,
                    color: active ? theme.text : theme.muted,
                  }}
                >
                  {name}
                </button>
              );
            })}
          </div>
        </div>

        {tab === "Home" && <HomeTab collection={collection} theme={theme} />}
        {tab === "Collection" && <CollectionTab collection={collection} query={query} setQuery={setQuery} theme={theme} />}
        {tab === "Wear Today" && <WearTodayTab collection={collection} theme={theme} />}
        {tab === "Layering" && <LayeringTab collection={collection} theme={theme} />}
        {tab === "Display" && <DisplayTab collection={collection} theme={theme} />}
        {tab === "Buy or Skip" && <BuyOrSkipTab collection={collection} theme={theme} />}
        {tab === "Analytics" && <AnalyticsTab collection={collection} theme={theme} />}
      </div>
    </div>
  );
}

export default function App() {
  try {
    return <SafeApp />;
  } catch (error) {
    return (
      <div style={{ padding: 24, fontFamily: "sans-serif" }}>
        <h1 style={{ fontSize: 28, marginBottom: 12 }}>The Vault</h1>
        <p style={{ marginBottom: 8 }}>The preview hit a runtime error, so the safe fallback loaded instead.</p>
        <pre style={{ whiteSpace: "pre-wrap", background: "#f3f3f3", padding: 12, borderRadius: 12 }}>
          {String(error)}
        </pre>
      </div>
    );
  }
}
