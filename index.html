// Kindred – SwiftUI layout (iOS 16+) · v4
// Works in Swift Playgrounds (App project) or Xcode: replace the contents of MyApp.swift with this file.
//
// What's new in v4
// • Reviews on every profile: star summary + rating breakdown, written reviews from other members
//   (what they did together, when, in person vs online) and a "Helpful" tap. Your own reviews from the
//   Connections tab show up on that person's profile too.
// • Discover cards show each member's rating and their most helpful review at a glance
// • Profile pictures that always show: a drawn portrait (no network needed) sits under the real photo,
//   so nobody is ever blank while loading or offline
// • Add your own photo: tap your picture on the Profile tab (PhotosPicker, saved on-device)
// • Profile tab now shows what people say about you; "Avg. rating" is the rating you receive
// • Redesigned profile sheet: bigger portrait, Match / Rating / In-common stats, About section,
//   and a Send-request button that stays pinned to the bottom
//
// What's new in v3
// • 26 members (was 10): 14 in Lisbon, 12 around the world
// • Real-looking profile pictures everywhere a person appears (cards, profile sheet, connections,
//   review sheet, hero banner, event attendees) – initials gradient shows while loading / offline
// • Each profile now has a photo gallery tied to their interests + city, with a swipeable full-screen viewer
// • Profile sheet opens full height so the gallery is visible straight away
// • NOTE: portraits come from randomuser.me and gallery photos from loremflickr.com (both need a network
//   connection). They're placeholders for the prototype – swap in your own CDN/user uploads before shipping.
//
// What's new in v2
// • Match score is now live: it blends personality compatibility with the interests you've selected,
//   so editing your profile really does "update your matches instantly"
// • State persists between launches (interests, requests, votes, reviews) via UserDefaults
// • Stable string IDs instead of per-launch UUIDs (needed for persistence, safer for diffing)
// • Vote counts derived from "everyone else's votes + yours" – no more +1/-1 drift
// • Accessibility: VoiceOver actions/values, Reduce Motion respected in every looping animation
// • Review sheet: 280-character limit with counter, "Update review" when editing
// • Single source of truth for the home city; filtered lists computed once per render
//
// Note: the Face ID screen is a visual simulation. Real identity verification between members needs a
// server-side ID/liveness provider – LocalAuthentication only proves the device owner is present.

import SwiftUI
import PhotosUI

// MARK: - App entry

@main
struct KindredApp: App {
    var body: some Scene { WindowGroup { RootView() } }
}

// MARK: - Config

enum Config {
    static let homeCity = "Lisbon"
    static let reviewLimit = 280
}

// MARK: - Theme

extension Color {
    init(hex: UInt32) {
        self.init(.sRGB,
                  red: Double((hex >> 16) & 0xFF) / 255,
                  green: Double((hex >> 8) & 0xFF) / 255,
                  blue: Double(hex & 0xFF) / 255,
                  opacity: 1)
    }
}

extension Font {
    /// Rounded system font – the one place the app's typography is defined.
    static func k(_ size: CGFloat, _ weight: Font.Weight = .regular) -> Font {
        .system(size: size, weight: weight, design: .rounded)
    }
}

extension String {
    /// "Maya Santos" -> "MS"
    var initials: String { split(separator: " ").compactMap { $0.first }.map(String.init).joined() }
}

enum Theme {
    static let ink = Color(hex: 0x2B2770)          // Deep Indigo
    static let inkLight = Color(hex: 0x5B50E0)     // Periwinkle
    static let lavender = Color(hex: 0xEEEAFE)     // Soft Lavender
    static let lavenderMid = Color(hex: 0xC9BEFF)
    static let coral = Color(hex: 0xFF7A6B)        // Warm Coral
    static let coralLight = Color(hex: 0xFF9E8A)
    static let coralSoft = Color(hex: 0xFFEAE6)
    static let mint = Color(hex: 0x34D399)         // Mint Green
    static let mintDark = Color(hex: 0x047857)
    static let canvas = Color(hex: 0xF7F6FC)       // Faintly tinted off-white
    static let slate = Color(hex: 0x6B7894)
    static let slateDark = Color(hex: 0x475569)
    static let muted = Color(hex: 0x9AA5BC)
    static let line = Color(hex: 0xE6E8F0)
    static let chipBg = Color(hex: 0xF1F2F9)
    static let star = Color(hex: 0xF5B301)

    static let brand = LinearGradient(colors: [ink, inkLight],
                                      startPoint: .topLeading, endPoint: .bottomTrailing)
    static let coralGradient = LinearGradient(colors: [coralLight, coral],
                                              startPoint: .top, endPoint: .bottom)

    private static let pairs: [(UInt32, UInt32)] = [
        (0x6366F1, 0xA78BFA), (0xFB7185, 0xF59E0B), (0x34D399, 0x0EA5E9), (0x8B5CF6, 0xEC4899)
    ]
    static func avatar(_ i: Int) -> LinearGradient {
        let p = pairs[i % pairs.count]
        return LinearGradient(colors: [Color(hex: p.0), Color(hex: p.1)],
                              startPoint: .topLeading, endPoint: .bottomTrailing)
    }
    static func avatarGlow(_ i: Int) -> Color {
        Color(hex: pairs[i % pairs.count].0).opacity(0.38)
    }
}

// MARK: - Models

struct Person: Identifiable {
    let name: String, age: Int, city: String, distance: String
    let code: String, type: String, bio: String
    let tags: [String]
    /// Personality-based compatibility (0–100). The displayed match also factors in shared interests.
    let compatibility: Int, tone: Int
    /// randomuser.me portrait path, e.g. "women/44" (change this one string to swap a face).
    let portrait: String

    var id: String { name }
    var initials: String { name.initials }
    var firstName: String { name.split(separator: " ").first.map(String.init) ?? name }
    var isLocal: Bool { city == Config.homeCity }

    func shared(with mine: Set<String>) -> Int { tags.filter { mine.contains($0) }.count }

    /// 70% personality compatibility + 30% share of this person's interests that you also have.
    func score(with mine: Set<String>) -> Int {
        guard !tags.isEmpty else { return compatibility }
        let overlap = Double(shared(with: mine)) / Double(tags.count)
        let blended = Double(compatibility) * 0.7 + overlap * 100 * 0.3
        return min(99, Int(blended.rounded()))
    }

    static let allInterests = ["Comics", "Movies", "Anime", "Gaming", "Board Games", "Books",
                               "Hiking", "Art", "Fitness", "Cooking", "Photography"]
    static let allTypes = ["Analyst", "Diplomat", "Sentinel", "Explorer"]

    private static let interestIcons: [String: String] = [
        "Comics": "text.bubble", "Movies": "film", "Anime": "tv", "Gaming": "gamecontroller",
        "Board Games": "dice", "Books": "book", "Hiking": "mountain.2", "Art": "paintpalette",
        "Fitness": "figure.run", "Cooking": "fork.knife", "Photography": "camera"
    ]
    private static let typeIcons: [String: String] = [
        "Analyst": "lightbulb", "Diplomat": "heart.circle", "Sentinel": "shield", "Explorer": "figure.walk"
    ]
    static func icon(for interest: String) -> String { interestIcons[interest] ?? "circle" }
    static func typeIcon(for type: String) -> String { typeIcons[type] ?? "sparkles" }

    // MARK: Photos

    static func portraitURL(_ path: String?) -> URL? {
        path.flatMap { URL(string: "https://randomuser.me/api/portraits/\($0).jpg") }
    }

    private static let photoKeywords: [String: String] = [
        "Comics": "comics", "Movies": "cinema", "Anime": "manga", "Gaming": "videogames",
        "Board Games": "boardgame", "Books": "bookstore", "Hiking": "hiking", "Art": "gallery",
        "Fitness": "running", "Cooking": "cooking", "Photography": "photography"
    ]

    /// Deterministic (String.hashValue changes every launch) so each person keeps the same photos.
    static func stableSeed(_ text: String) -> Int {
        text.unicodeScalars.reduce(7) { ($0 &* 31 &+ Int($1.value)) % 9_973 }
    }

    /// Up to three photos matching their interests, plus one of their city.
    var photoURLs: [URL] {
        var keys = tags.prefix(3).compactMap { Self.photoKeywords[$0] }
        keys.append(city.folding(options: .diacriticInsensitive, locale: .current)
            .lowercased().filter { $0.isLetter })
        let seed = Self.stableSeed(name)
        return keys.enumerated().compactMap { i, key in
            URL(string: "https://loremflickr.com/640/800/\(key)?lock=\(seed + i * 101)")
        }
    }

    static let samples: [Person] = [
        Person(name: "Maya Santos", age: 27, city: "Lisbon", distance: "2 km away", code: "INFJ", type: "Diplomat",
               bio: "Weekend comic-shop wanderer. Looking for someone to split a Con weekend with.",
               tags: ["Comics", "Movies", "Art"], compatibility: 92, tone: 0, portrait: "women/44"),
        Person(name: "Daniel Okafor", age: 31, city: "Lisbon", distance: "4 km away", code: "INTP", type: "Analyst",
               bio: "Board game café regular. I'll teach you Catan, you teach me patience.",
               tags: ["Board Games", "Gaming", "Books"], compatibility: 84, tone: 1, portrait: "men/32"),
        Person(name: "Priya Raman", age: 33, city: "Lisbon", distance: "6 km away", code: "ENFJ", type: "Diplomat",
               bio: "Trail runner, new in town. Sunday hikes followed by pastries.",
               tags: ["Hiking", "Fitness", "Cooking"], compatibility: 79, tone: 2, portrait: "women/68"),
        Person(name: "Tomás Pereira", age: 29, city: "Lisbon", distance: "3 km away", code: "ISFJ", type: "Sentinel",
               bio: "Anime nights and ramen runs. Always up for a convention road trip.",
               tags: ["Anime", "Gaming", "Movies"], compatibility: 76, tone: 3, portrait: "men/75"),
        Person(name: "Aisha Khan", age: 26, city: "Lisbon", distance: "5 km away", code: "ESTP", type: "Explorer",
               bio: "Photographer chasing golden hour. Bring a book, I'll bring the camera.",
               tags: ["Photography", "Hiking", "Books"], compatibility: 73, tone: 1, portrait: "women/26"),
        Person(name: "Lucas Meyer", age: 35, city: "Lisbon", distance: "7 km away", code: "ISTJ", type: "Sentinel",
               bio: "Cooks for friends, plays strategy games, hosts a monthly movie night.",
               tags: ["Board Games", "Movies", "Cooking"], compatibility: 70, tone: 2, portrait: "men/46"),
        Person(name: "Kenji Tanaka", age: 30, city: "Tokyo", distance: "9,800 km away", code: "INTJ", type: "Analyst",
               bio: "Manga collector and retro-game tinkerer. Happy to swap recommendations online.",
               tags: ["Anime", "Comics", "Gaming"], compatibility: 88, tone: 3, portrait: "men/22"),
        Person(name: "Lena Fischer", age: 28, city: "Berlin", distance: "2,300 km away", code: "ENFP", type: "Explorer",
               bio: "Museum hopper and weekend hiker. Planning a Lisbon trip next spring.",
               tags: ["Hiking", "Movies", "Art"], compatibility: 81, tone: 0, portrait: "women/65"),
        Person(name: "Sofia Rossi", age: 32, city: "Rome", distance: "1,800 km away", code: "ESFJ", type: "Sentinel",
               bio: "Book club founder who cooks too much pasta. Let's trade reading lists.",
               tags: ["Books", "Cooking", "Board Games"], compatibility: 77, tone: 1, portrait: "women/33"),
        Person(name: "Ben Carter", age: 34, city: "Toronto", distance: "5,700 km away", code: "ENTP", type: "Analyst",
               bio: "Runs a weekly online game night. Comics, co-op, and terrible puns.",
               tags: ["Comics", "Gaming", "Board Games"], compatibility: 74, tone: 2, portrait: "men/52"),
        Person(name: "Inês Carvalho", age: 28, city: "Lisbon", distance: "1 km away", code: "ISFP", type: "Explorer",
               bio: "Ceramics on weekdays, tile-hunting on weekends. Always up for a gallery crawl in Alfama.",
               tags: ["Art", "Photography", "Cooking"], compatibility: 86, tone: 2, portrait: "women/79"),
        Person(name: "Rafael Costa", age: 32, city: "Lisbon", distance: "3 km away", code: "ENTJ", type: "Analyst",
               bio: "Runs a Thursday tabletop RPG night above a bookshop in Príncipe Real. We always need one more player.",
               tags: ["Gaming", "Board Games", "Books"], compatibility: 82, tone: 3, portrait: "men/86"),
        Person(name: "Mei Lin Wong", age: 25, city: "Lisbon", distance: "4 km away", code: "INFP", type: "Diplomat",
               bio: "Illustrator who lives in sketchbooks. Looking for cinema buddies and anyone who loves a good webcomic.",
               tags: ["Comics", "Art", "Movies"], compatibility: 90, tone: 1, portrait: "women/90"),
        Person(name: "Jonas Berg", age: 36, city: "Lisbon", distance: "8 km away", code: "ISTP", type: "Explorer",
               bio: "Swedish expat. Surf at dawn, long hikes after. Will trade surf tips for good coffee spots.",
               tags: ["Hiking", "Fitness", "Photography"], compatibility: 71, tone: 0, portrait: "men/11"),
        Person(name: "Camila Duarte", age: 30, city: "Lisbon", distance: "2 km away", code: "ENFJ", type: "Diplomat",
               bio: "Cooks for twelve on a good Sunday. Anime marathons and a very big pot of caldo verde.",
               tags: ["Cooking", "Anime", "Movies"], compatibility: 78, tone: 3, portrait: "women/17"),
        Person(name: "Omar Haddad", age: 29, city: "Lisbon", distance: "5 km away", code: "INTP", type: "Analyst",
               bio: "Developer by day, retro-console collector by night. Currently hunting for a working SNES.",
               tags: ["Gaming", "Anime", "Comics"], compatibility: 80, tone: 2, portrait: "men/15"),
        Person(name: "Beatriz Nunes", age: 34, city: "Lisbon", distance: "6 km away", code: "ESTJ", type: "Sentinel",
               bio: "Bookseller organising a monthly silent-reading night at a café in Graça.",
               tags: ["Books", "Movies", "Board Games"], compatibility: 68, tone: 1, portrait: "women/47"),
        Person(name: "Gabriel Martins", age: 26, city: "Lisbon", distance: "3 km away", code: "ESFP", type: "Explorer",
               bio: "Climbing-gym regular, learning to cook from scratch, and a menace at anime-opening karaoke.",
               tags: ["Fitness", "Anime", "Cooking"], compatibility: 75, tone: 0, portrait: "men/68"),
        Person(name: "Oliver Hughes", age: 33, city: "London", distance: "1,650 km away", code: "ISTJ", type: "Sentinel",
               bio: "Hosts a weekly video-call game night. Always happy to teach a new board game.",
               tags: ["Board Games", "Gaming", "Movies"], compatibility: 75, tone: 1, portrait: "men/45"),
        Person(name: "Zoe Alvarez", age: 27, city: "Mexico City", distance: "9,500 km away", code: "ENFJ", type: "Diplomat",
               bio: "Zines, lucha libre posters and comic cons. Always scheming a trip to Europe.",
               tags: ["Comics", "Art", "Photography"], compatibility: 87, tone: 3, portrait: "women/21"),
        Person(name: "Min-jun Park", age: 29, city: "Seoul", distance: "10,000 km away", code: "ENTJ", type: "Analyst",
               bio: "Webtoon editor who plays co-op games after work. Swaps recommendations at any hour.",
               tags: ["Comics", "Gaming", "Anime"], compatibility: 85, tone: 2, portrait: "men/62"),
        Person(name: "Chloe Bennett", age: 31, city: "Sydney", distance: "17,500 km away", code: "ESFP", type: "Explorer",
               bio: "Coastal walker and amateur photographer. Dawn swims, sunset hikes, long-distance book swaps.",
               tags: ["Hiking", "Photography", "Books"], compatibility: 76, tone: 0, portrait: "women/55"),
        Person(name: "Thandiwe Mokoena", age: 28, city: "Cape Town", distance: "8,400 km away", code: "INFP", type: "Diplomat",
               bio: "Film-school grad writing her first feature. Trading watchlists and favourite scripts.",
               tags: ["Movies", "Books", "Art"], compatibility: 83, tone: 1, portrait: "women/12"),
        Person(name: "Mateus Almeida", age: 30, city: "São Paulo", distance: "7,900 km away", code: "ESFJ", type: "Sentinel",
               bio: "Feijoada host, Brazilian comics fan and board game evangelist.",
               tags: ["Cooking", "Comics", "Board Games"], compatibility: 72, tone: 3, portrait: "men/85"),
        Person(name: "Anouk de Vries", age: 32, city: "Amsterdam", distance: "1,850 km away", code: "ISFJ", type: "Sentinel",
               bio: "Cyclist and bookbinder. Happy to swap reading lists, letters and weekend rides.",
               tags: ["Books", "Art", "Fitness"], compatibility: 74, tone: 2, portrait: "women/63"),
        Person(name: "Noah Sullivan", age: 35, city: "New York", distance: "5,400 km away", code: "ISTP", type: "Explorer",
               bio: "Trail runner and film nerd. Hosts a monthly 'so bad it's good' movie night over video call.",
               tags: ["Fitness", "Movies", "Hiking"], compatibility: 70, tone: 0, portrait: "men/19")
    ]
}

struct EventItem: Identifiable {
    let day: String, month: String, title: String, place: String
    /// Votes from everyone else; your own vote is added on top when `voted` is true.
    let baseVotes: Int
    let going: Int
    var voted: Bool

    var id: String { title }
    var votes: Int { baseVotes + (voted ? 1 : 0) }

    static let featured = EventItem(day: "16", month: "NOV", title: "Lisbon Comic Meet-Up",
                                    place: "Casa do Comum", baseVotes: 127, going: 24, voted: true)
    static let samples: [EventItem] = [
        EventItem(day: "23", month: "NOV", title: "Ghibli Watch Party", place: "Cinema Ideal",
                  baseVotes: 93, going: 31, voted: true),
        EventItem(day: "30", month: "NOV", title: "Board Game Saturday", place: "Taberna Dados",
                  baseVotes: 71, going: 18, voted: false),
        EventItem(day: "07", month: "DEC", title: "Sunset Hike · Sintra", place: "Pena Park",
                  baseVotes: 58, going: 12, voted: false)
    ]
}

struct Connection: Identifiable {
    let name: String, detail: String, tone: Int
    let portrait: String
    var rating: Int?
    var review: String? = nil

    var id: String { name }
    var initials: String { name.initials }
    var firstName: String { name.split(separator: " ").first.map(String.init) ?? name }

    static let samples: [Connection] = [
        Connection(name: "Maya Santos", detail: "Comic Meet-Up · Oct 12", tone: 0, portrait: "women/44", rating: nil),
        Connection(name: "Priya Raman", detail: "Sunday hike · Oct 5", tone: 2, portrait: "women/68", rating: 5),
        Connection(name: "Mateo Vargas", detail: "Board game night · Sep 28", tone: 3, portrait: "men/41", rating: 4)
    ]
}

// MARK: - Reviews

struct Review: Identifiable {
    var id: String
    let author: String
    /// randomuser.me portrait path for the reviewer (empty for "You").
    let portrait: String
    let rating: Int
    let text: String
    /// What they did together, e.g. "Sunday hike".
    let context: String
    let when: String
    /// How many members found this review helpful (before your own tap).
    let helpful: Int
    var isYou = false

    var authorFirst: String { author.split(separator: " ").first.map(String.init) ?? author }
}

struct RatingSummary {
    let count: Int
    let average: Double
    /// Index 0 = 1 star … index 4 = 5 stars.
    let histogram: [Int]

    init(_ reviews: [Review]) {
        count = reviews.count
        average = reviews.isEmpty ? 0 : Double(reviews.map(\.rating).reduce(0, +)) / Double(reviews.count)
        histogram = (1...5).map { star in reviews.filter { $0.rating == star }.count }
    }

    var label: String { count == 0 ? "New" : String(format: "%.1f", average) }
}

/// Hand-written sample reviews. Mixed ratings and honest, specific comments so profiles feel lived-in.
/// Swap for server data later – `Store.profileReviews(for:)` is the only place that reads this.
enum ReviewBank {
    static func reviews(for id: String) -> [Review] { all[id] ?? [] }

    private static func r(_ author: String, _ portrait: String, _ rating: Int, _ context: String,
                          _ when: String, _ text: String, _ helpful: Int = 0) -> Review {
        Review(id: author, author: author, portrait: portrait, rating: rating, text: text,
               context: context, when: when, helpful: helpful)
    }

    private static let all: [String: [Review]] = {
        var d: [String: [Review]] = [:]

        d["Maya Santos"] = [
            r("Hana Ito", "women/9", 5, "Comic Meet-Up", "2 weeks ago",
              "Maya found the one stall with the out-of-print issues I had been hunting for years. Easy company, and she remembered everything I said about my favourite artists.", 14),
            r("Diogo Ferreira", "men/7", 5, "Cinema Ideal", "1 month ago",
              "Went to a Ghibli double feature together. We kept talking through dinner and ended up staying until the staff were stacking chairs.", 9),
            r("Sam Whitaker", "men/29", 4, "Art walk", "2 months ago",
              "Lovely person with strong opinions about every gallery we visited, which I enjoyed. We started a bit late because of the tram, but she messaged ahead.", 3),
            r("Rita Gomes", "women/36", 5, "Comic Meet-Up", "3 months ago",
              "Felt comfortable straight away. She introduced me to her friends and made sure I wasn't standing alone at the table.", 6)
        ]
        d["Daniel Okafor"] = [
            r("Marta Silva", "women/5", 5, "Taberna Dados", "3 weeks ago",
              "Taught our whole table Catan with endless patience. Never once made anyone feel slow.", 11),
            r("Pedro Lopes", "men/3", 4, "Game night", "6 weeks ago",
              "Great host and very sharp at strategy games. The one catch is that he takes a long time on his turn. Still a good evening.", 7),
            r("Emma Clarke", "women/14", 5, "Board Game Saturday", "2 months ago",
              "I had never played anything beyond Monopoly. He started me on Azul and I have since bought it. Would happily lose to him again.", 8)
        ]
        d["Priya Raman"] = [
            r("João Mendes", "men/9", 5, "Sunday hike", "1 week ago",
              "Pace was perfect and she brought a flask of chai for the whole group. The pastry stop in Belém afterwards was the best part.", 18),
            r("Laura Pinto", "women/24", 5, "Trail run", "3 weeks ago",
              "Newish to the city and she still knew the best routes. Waited at the top for me twice without a hint of impatience.", 10),
            r("Chris Dalton", "men/36", 4, "Sunday hike", "2 months ago",
              "Brilliant company. The route was a bit longer than advertised, so bring snacks.", 5)
        ]
        d["Tomás Pereira"] = [
            r("Rui Barbosa", "men/4", 5, "Anime night", "2 weeks ago",
              "Hosted at his place with far too much food. Great taste in series and he never spoiled a single plot twist.", 7),
            r("Ana Teixeira", "women/8", 4, "Ramen run", "5 weeks ago",
              "Fun and chatty. Took us to a ramen place I would never have found. A bit quiet at first, warms up quickly.", 4),
            r("Kai Nakamura", "men/13", 5, "Convention road trip", "3 months ago",
              "Shared a car to the Amadora comic fair. Good playlist, good snacks, zero drama.", 9)
        ]
        d["Aisha Khan"] = [
            r("Nuno Ribeiro", "men/16", 5, "Golden hour walk", "10 days ago",
              "Aisha spotted light I would have walked straight past. She gave me a few quick framing tips without making it feel like a lesson.", 13),
            r("Clara Vidal", "women/30", 5, "Miradouro meetup", "1 month ago",
              "Showed up with a thermos and a spare camera strap because mine had broken. Thoughtful and a lot of fun.", 12),
            r("Tim Brandt", "men/18", 4, "Photo walk", "2 months ago",
              "Great eye, great conversation. We lost the light early so it turned into a café stop, which I didn't mind.", 2)
        ]
        d["Lucas Meyer"] = [
            r("Sara Antunes", "women/2", 5, "Movie night", "3 weeks ago",
              "His monthly movie night is properly organised: a theme, a shared playlist before the film and a very good lasagne.", 8),
            r("Henrik Olsen", "men/20", 4, "Strategy night", "7 weeks ago",
              "Welcoming host. Rules explanations run long, but he checks in to see if everyone is keeping up.", 5),
            r("Joana Reis", "women/13", 5, "Movie night", "3 months ago",
              "Warm without being over the top. I came alone and left having made two new friends.", 10)
        ]
        d["Kenji Tanaka"] = [
            r("Felipe Rocha", "men/23", 5, "Manga swap", "2 weeks ago",
              "Sent me a whole reading list with notes on where to start each series. Time zones are hard and he still stayed up for the call.", 9),
            r("Nina Petrova", "women/3", 4, "Retro gaming chat", "6 weeks ago",
              "Really knowledgeable and generous with recommendations. Replies can take a day, which makes sense with the time difference.", 3),
            r("Alex Moreau", "men/10", 5, "Co-op night", "2 months ago",
              "Patient while teaching me a game he has obviously played hundreds of times.", 6)
        ]
        d["Lena Fischer"] = [
            r("Isabel Moura", "women/6", 5, "Video call", "1 week ago",
              "Warm, curious, asks real questions. We have already sketched out a gallery crawl for when she visits Lisbon.", 7),
            r("Dev Patel", "men/12", 5, "Film chat", "5 weeks ago",
              "Her recommendations were spot on. My watchlist is now far too long, thanks Lena.", 4),
            r("Mia Larsen", "women/19", 4, "Online hangout", "3 months ago",
              "Easy to talk to and very enthusiastic. A bit chaotic about scheduling, but she makes up for it.", 2)
        ]
        d["Sofia Rossi"] = [
            r("Teresa Faria", "women/1", 5, "Book swap", "3 weeks ago",
              "She mailed me a novel she loved with a handwritten note inside. Lovely gesture.", 15),
            r("Michael Doyle", "men/24", 5, "Online book club", "6 weeks ago",
              "Runs the discussion really well. Keeps it friendly and makes sure the quieter people get a say.", 8),
            r("Elif Aydin", "women/28", 4, "Reading list trade", "2 months ago",
              "Great taste. A few of her picks were heavier than I was in the mood for, but I'm glad I read them.", 3)
        ]
        d["Ben Carter"] = [
            r("Gustavo Lima", "men/25", 5, "Online game night", "2 weeks ago",
              "The puns are terrible and I looked forward to every one. Good at making sure nobody gets left out of co-op games.", 10),
            r("Priscila Rangel", "women/20", 4, "Game night", "5 weeks ago",
              "Fun group and well organised. It starts a bit later than planned, so don't panic if you are alone on the call for ten minutes.", 6),
            r("Owen Price", "men/14", 5, "Comics chat", "3 months ago",
              "Pulled up half a dozen indie titles I had never heard of. Real enthusiast.", 4)
        ]
        d["Inês Carvalho"] = [
            r("Carolina Matos", "women/16", 5, "Gallery crawl", "1 week ago",
              "Took me to galleries I would never have spotted alone and knew the owners by name. Genuinely kind.", 12),
            r("Vasco Neves", "men/6", 5, "Ceramics studio", "1 month ago",
              "Showed me how to throw a wobbly little bowl. Lots of laughing, zero pressure.", 9),
            r("Hannah Fox", "women/32", 4, "Tile hunt", "2 months ago",
              "Lovely day wandering for azulejos. She walks fast, so wear comfortable shoes.", 5)
        ]
        d["Rafael Costa"] = [
            r("Leonor Cabral", "women/10", 5, "Thursday RPG night", "10 days ago",
              "First time ever playing tabletop. He kept it welcoming and my terrible character became everyone's favourite.", 16),
            r("Tiago Sá", "men/2", 5, "Thursday RPG night", "5 weeks ago",
              "Excellent game master. Good pacing, great voices, and the bookshop setting is lovely.", 11),
            r("Ellie Warren", "women/22", 4, "Board games", "2 months ago",
              "Warm and organised. The session ran past midnight, which was great fun but plan for a late night.", 4),
            r("Jorge Pacheco", "men/17", 5, "RPG night", "4 months ago",
              "Always asks if anyone needs a break. Rare in a GM.", 7)
        ]
        d["Mei Lin Wong"] = [
            r("Ricardo Alves", "men/8", 5, "Cinema", "2 weeks ago",
              "Mei Lin noticed details in the film I had completely missed. We swapped webcomic links all the way home.", 8),
            r("Sofia Gama", "women/4", 5, "Sketch meetup", "6 weeks ago",
              "Brought extra pencils and a spare sketchbook for me. Quiet at first but really funny once she settles in.", 11),
            r("Jamal Reed", "men/21", 4, "Webcomic chat", "3 months ago",
              "Friendly and creative. We only had an hour and I would happily have stayed longer.", 3)
        ]
        d["Jonas Berg"] = [
            r("Patrícia Dias", "women/18", 4, "Sunrise surf", "3 weeks ago",
              "Very patient teaching a beginner and knows the beaches well. Not much of a talker before coffee, fair warning.", 9),
            r("Lukas Weber", "men/26", 5, "Sintra hike", "6 weeks ago",
              "Laid-back, reliable, and kept the whole group safe on a slippery trail. Good photos too.", 7),
            r("Anna Kowalski", "women/37", 4, "Coffee meetup", "2 months ago",
              "Gave me a list of coffee spots I am still working through. Straightforward and kind.", 3)
        ]
        d["Camila Duarte"] = [
            r("Hugo Santos", "men/5", 5, "Sunday lunch", "2 weeks ago",
              "The caldo verde was unreal and she made sure everyone had seconds. Felt like being invited to a friend's family lunch.", 17),
            r("Yara Costa", "women/7", 5, "Anime marathon", "5 weeks ago",
              "Comfortable sofas, snacks everywhere and zero judgement about how slowly I watch.", 8),
            r("Rob Lindqvist", "men/30", 4, "Sunday lunch", "3 months ago",
              "Warm host. It was quite a crowd, so lively rather than quiet. Great if that is your thing.", 4)
        ]
        d["Omar Haddad"] = [
            r("Filipe Cunha", "men/1", 5, "Retro game night", "1 week ago",
              "Hooked up a CRT and a whole shelf of cartridges. Properly nerdy in the best way.", 12),
            r("Maria Esteves", "women/11", 4, "Coffee chat", "5 weeks ago",
              "Interesting and funny. Gets very deep into console trivia, so be ready to nod a lot.", 6),
            r("Ivan Petrov", "men/33", 5, "SNES hunt", "2 months ago",
              "Went flea-market hunting with him and he haggled like a pro. Great day out.", 5)
        ]
        d["Beatriz Nunes"] = [
            r("Margarida Lobo", "women/15", 5, "Silent reading night", "2 weeks ago",
              "Calm, cosy and perfectly organised. Exactly what I needed after a loud week.", 13),
            r("Daniel Kerr", "men/27", 4, "Silent reading night", "7 weeks ago",
              "Lovely idea and she runs it well. A bit strict about the no-talking rule, but that is the point.", 6),
            r("Carla Brito", "women/25", 5, "Bookshop visit", "3 months ago",
              "Recommended three books and every one landed.", 9)
        ]
        d["Gabriel Martins"] = [
            r("Miguel Torres", "men/28", 5, "Climbing gym", "10 days ago",
              "Super encouraging. Spotted me on my first bouldering attempt and never made me feel like a beginner.", 10),
            r("Lucie Bernard", "women/29", 4, "Anime karaoke", "1 month ago",
              "Hilarious and high energy. Probably not the one to pick if you want a quiet night.", 8),
            r("Alexei Volkov", "men/31", 5, "Cooking night", "2 months ago",
              "We attempted risotto from scratch. It was a disaster and one of the best evenings I have had this year.", 12)
        ]
        d["Oliver Hughes"] = [
            r("Jess Cooper", "women/35", 5, "Video game night", "2 weeks ago",
              "Explained a new board game in under five minutes and kept things moving. Great vibe.", 6),
            r("Noel Brennan", "men/34", 4, "Online game night", "5 weeks ago",
              "Friendly and well prepared. His connection dropped once, but it sorted itself out quickly.", 2),
            r("Zainab Ali", "women/38", 5, "Movie chat", "3 months ago",
              "Easy conversation and good recommendations.", 4)
        ]
        d["Zoe Alvarez"] = [
            r("Daniela Cruz", "women/39", 5, "Zine swap", "1 week ago",
              "She posted me three zines and a lucha libre sticker pack. I am now a fan. Funny and generous.", 14),
            r("Marc Dubois", "men/37", 5, "Comic con chat", "6 weeks ago",
              "Full of convention stories. We already have a plan to meet if she makes it to Europe.", 7),
            r("Peter Novak", "men/38", 4, "Video call", "2 months ago",
              "Warm and chatty. The time difference means late evenings, but worth it.", 3)
        ]
        d["Min-jun Park"] = [
            r("Rebeca Sousa", "women/40", 5, "Webtoon swap", "3 weeks ago",
              "Gave me honest, useful feedback on a webcomic I am drawing. Kind, but not just flattering.", 15),
            r("Gareth Lloyd", "men/39", 5, "Co-op night", "5 weeks ago",
              "Very good teammate who stays calm and communicates well under pressure.", 6),
            r("Sunita Rao", "women/41", 4, "Anime chat", "2 months ago",
              "Interesting taste and quick replies. Gets deep into editing talk, which I enjoyed.", 3)
        ]
        d["Chloe Bennett"] = [
            r("Tomasz Wolski", "men/40", 5, "Book swap", "2 weeks ago",
              "Shipped me a paperback with photos of her favourite coastal walks tucked inside. Thoughtful.", 11),
            r("Eva Lindgren", "women/42", 4, "Video call", "6 weeks ago",
              "Cheerful and easy to chat with. Dawn calls are tough on Lisbon time, but she was flexible.", 4),
            r("Pablo Ortiz", "men/48", 5, "Photo chat", "3 months ago",
              "Talked me through her favourite shots and why she took them. Inspiring.", 6)
        ]
        d["Thandiwe Mokoena"] = [
            r("Lucía Navarro", "women/43", 5, "Script swap", "10 days ago",
              "Sent me pages of her feature to read and was generous about my notes. Smart, funny and gracious.", 13),
            r("Callum Reid", "men/42", 5, "Watchlist trade", "1 month ago",
              "Her recommendations were all excellent. Now I owe her a list.", 7),
            r("Dina Moreira", "women/45", 4, "Video call", "3 months ago",
              "Warm and engaging. The call ran over because we couldn't stop talking about films.", 4)
        ]
        d["Mateus Almeida"] = [
            r("Beto Carvalho", "men/43", 5, "Board game call", "3 weeks ago",
              "Came with a feijoada recipe and a board game evangelism speech I wasn't expecting to enjoy. I did.", 9),
            r("Sandra Quaresma", "women/46", 4, "Comics chat", "6 weeks ago",
              "Friendly, generous and very passionate about Brazilian comics. Sends voice notes rather than texts.", 5),
            r("Luc Martin", "men/44", 5, "Online game night", "2 months ago",
              "Welcoming to newcomers.", 3)
        ]
        d["Anouk de Vries"] = [
            r("Greta Hoffmann", "women/48", 5, "Letter swap", "2 weeks ago",
              "Her handwritten letters are works of art. Thoughtful, kind and surprisingly funny.", 12),
            r("Jelle Visser", "men/49", 4, "Reading list swap", "5 weeks ago",
              "Brilliant recommendations. Replies take a few days, which suits the slow-letters idea.", 3),
            r("Naomi Cole", "women/49", 5, "Book chat", "3 months ago",
              "The bookbinding talk was fascinating.", 6)
        ]
        d["Noah Sullivan"] = [
            r("Eddie Marsh", "men/51", 5, "Bad movie night", "1 week ago",
              "The worst film we have ever picked and one of the best nights. A brilliant host over video.", 9),
            r("Katya Ivanova", "women/50", 4, "Movie night", "5 weeks ago",
              "Funny and warm. A couple of his picks were truly awful, as advertised.", 7),
            r("Paulo Ramos", "men/47", 5, "Trail chat", "2 months ago",
              "Great tips on trail running gear and no ego about it.", 4)
        ]
        // Reviews about the signed-in user (shown on the Profile tab)
        d["You"] = [
            r("Maya Santos", "women/44", 5, "Comic Meet-Up", "3 weeks ago",
              "Thoughtful, funny and easy to talk to. Made me feel at ease right away.", 5),
            r("Priya Raman", "women/68", 5, "Sunday hike", "Oct 5",
              "Great hiking buddy. Kept a steady pace and shared snacks.", 3),
            r("Mateo Vargas", "men/41", 4, "Board game night", "Sep 28",
              "Fun evening and very competitive in the best way. Would play again.", 2)
        ]

        var out: [String: [Review]] = [:]
        for (key, list) in d {
            out[key] = list.map { review in
                var copy = review
                copy.id = "\(key)#\(review.author)"
                return copy
            }
        }
        return out
    }()
}

// MARK: - Shared app state (all interactions flow through here)

@MainActor
final class Store: ObservableObject {
    private struct StoredReview: Codable { var rating: Int; var text: String? }
    private struct Snapshot: Codable {
        var interests: [String]
        var requested: [String]
        var voted: [String]
        var reviews: [String: StoredReview]
        var helpful: [String]? = nil   // optional so v3 saves still decode
    }

    private static let storageKey = "kindred.state.v1"
    private let defaults: UserDefaults

    @Published private(set) var myInterests: Set<String> = ["Comics", "Movies", "Board Games", "Hiking"]
    @Published private(set) var requested: Set<String> = [Person.samples[1].id]
    @Published private(set) var votedEvents: Set<String> =
        Set(([EventItem.featured] + EventItem.samples).filter(\.voted).map(\.id))
    @Published private var reviews: [String: StoredReview] = [:]
    @Published private(set) var helpful: Set<String> = []
    @Published private(set) var myPhoto: UIImage? = nil

    init(defaults: UserDefaults = .standard) {
        self.defaults = defaults
        myPhoto = UIImage(contentsOfFile: Self.photoURL.path)
        guard let data = defaults.data(forKey: Self.storageKey),
              let snap = try? JSONDecoder().decode(Snapshot.self, from: data) else { return }
        myInterests = Set(snap.interests).intersection(Person.allInterests)
        requested = Set(snap.requested)
        votedEvents = Set(snap.voted)
        reviews = snap.reviews
        helpful = Set(snap.helpful ?? [])
    }

    // Derived views of the data
    var featured: EventItem { resolve(EventItem.featured) }
    var events: [EventItem] { EventItem.samples.map(resolve) }

    var connections: [Connection] {
        Connection.samples.map { c in
            guard let r = reviews[c.id] else { return c }
            var copy = c
            copy.rating = r.rating
            copy.review = r.text
            return copy
        }
    }

    var pendingReviews: Int { connections.filter { $0.rating == nil }.count }
    var votedCount: Int { votedEvents.count }
    var averageRating: Double? {
        let r = connections.compactMap { $0.rating }
        return r.isEmpty ? nil : Double(r.reduce(0, +)) / Double(r.count)
    }

    // Actions
    func toggleInterest(_ tag: String) {
        if myInterests.contains(tag) { myInterests.remove(tag) } else { myInterests.insert(tag) }
        save()
    }

    func toggleRequest(_ person: Person) {
        if requested.contains(person.id) { requested.remove(person.id) } else { requested.insert(person.id) }
        save()
    }

    func toggleVote(_ event: EventItem) {
        if votedEvents.contains(event.id) { votedEvents.remove(event.id) } else { votedEvents.insert(event.id) }
        save()
    }

    func submitReview(for id: String, rating: Int, text: String) {
        reviews[id] = StoredReview(rating: rating, text: text.isEmpty ? nil : text)
        save()
    }

    // Reviews shown on profiles
    /// Other members' reviews, preceded by yours if you've reviewed this person from Connections.
    func profileReviews(for person: Person) -> [Review] {
        var list = ReviewBank.reviews(for: person.id)
        if let c = connections.first(where: { $0.id == person.id }), let rating = c.rating {
            let parts = c.detail.components(separatedBy: " · ")
            list.insert(Review(id: "\(person.id)#you", author: "You", portrait: "", rating: rating,
                               text: c.review ?? "", context: parts.first ?? c.detail,
                               when: parts.count > 1 ? parts[1] : "Recently", helpful: 0, isYou: true), at: 0)
        }
        return list
    }

    var reviewsAboutMe: [Review] { ReviewBank.reviews(for: "You") }

    func isHelpful(_ id: String) -> Bool { helpful.contains(id) }

    func toggleHelpful(_ id: String) {
        if helpful.contains(id) { helpful.remove(id) } else { helpful.insert(id) }
        save()
    }

    // Your own profile photo (stored as a small JPEG in Documents)
    private static var photoURL: URL {
        FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)[0]
            .appendingPathComponent("kindred-me.jpg")
    }

    func setPhoto(_ data: Data) {
        guard let source = UIImage(data: data) else { return }
        let scale = min(1, 640 / max(source.size.width, source.size.height))
        let size = CGSize(width: source.size.width * scale, height: source.size.height * scale)
        let format = UIGraphicsImageRendererFormat()
        format.scale = 1
        let image = UIGraphicsImageRenderer(size: size, format: format).image { _ in
            source.draw(in: CGRect(origin: .zero, size: size))
        }
        if let jpeg = image.jpegData(compressionQuality: 0.85) { try? jpeg.write(to: Self.photoURL, options: .atomic) }
        myPhoto = image
    }

    func removePhoto() {
        try? FileManager.default.removeItem(at: Self.photoURL)
        myPhoto = nil
    }

    // Helpers
    private func resolve(_ event: EventItem) -> EventItem {
        var e = event
        e.voted = votedEvents.contains(event.id)
        return e
    }

    private func save() {
        let snap = Snapshot(interests: Array(myInterests), requested: Array(requested),
                            voted: Array(votedEvents), reviews: reviews, helpful: Array(helpful))
        if let data = try? JSONEncoder().encode(snap) { defaults.set(data, forKey: Self.storageKey) }
    }
}

enum Haptics {
    static func tap() { UISelectionFeedbackGenerator().selectionChanged() }
    static func success() { UINotificationFeedbackGenerator().notificationOccurred(.success) }
}

// MARK: - Reusable pieces

extension View {
    /// White card with a hairline border and a soft, two-layer shadow.
    func cardChrome(radius: CGFloat = 22) -> some View {
        self.background(Color.white)
            .clipShape(RoundedRectangle(cornerRadius: radius, style: .continuous))
            .overlay(RoundedRectangle(cornerRadius: radius, style: .continuous)
                .strokeBorder(Theme.ink.opacity(0.05), lineWidth: 1))
            .shadow(color: Theme.ink.opacity(0.05), radius: 2, y: 1)
            .shadow(color: Theme.ink.opacity(0.10), radius: 20, y: 10)
    }
    func card(padding: CGFloat = 16) -> some View { self.padding(padding).cardChrome() }
}

struct PressStyle: ButtonStyle {
    func makeBody(configuration: Configuration) -> some View {
        configuration.label
            .scaleEffect(configuration.isPressed ? 0.95 : 1)
            .opacity(configuration.isPressed ? 0.9 : 1)
            .animation(.spring(response: 0.25, dampingFraction: 0.7), value: configuration.isPressed)
    }
}

/// Faint lavender + coral glow behind the content of every screen.
struct AmbientBackground: View {
    var body: some View {
        ZStack(alignment: .top) {
            Theme.canvas
            Circle().fill(Theme.lavenderMid.opacity(0.45))
                .frame(width: 320, height: 320).blur(radius: 70).offset(x: -140, y: -130)
            Circle().fill(Theme.coral.opacity(0.16))
                .frame(width: 260, height: 260).blur(radius: 70).offset(x: 150, y: -70)
        }
        .ignoresSafeArea()
        .accessibilityHidden(true)
    }
}

struct HairLine: View {
    var body: some View { Rectangle().fill(Theme.line.opacity(0.8)).frame(height: 1) }
}

/// Wraps its children onto new lines (iOS 16 Layout protocol).
struct FlowLayout: Layout {
    var spacing: CGFloat = 8
    var lineSpacing: CGFloat = 8

    func sizeThatFits(proposal: ProposedViewSize, subviews: Subviews, cache: inout ()) -> CGSize {
        let maxW = proposal.width ?? .infinity
        var x: CGFloat = 0, y: CGFloat = 0, rowH: CGFloat = 0, width: CGFloat = 0
        for s in subviews {
            let sz = s.sizeThatFits(.unspecified)
            if x > 0 && x + sz.width > maxW {
                y += rowH + lineSpacing
                x = 0
                rowH = 0
            }
            x += sz.width + spacing
            rowH = max(rowH, sz.height)
            width = max(width, x - spacing)
        }
        return CGSize(width: width, height: y + rowH)
    }

    func placeSubviews(in bounds: CGRect, proposal: ProposedViewSize, subviews: Subviews, cache: inout ()) {
        var x = bounds.minX, y = bounds.minY, rowH: CGFloat = 0
        for s in subviews {
            let sz = s.sizeThatFits(.unspecified)
            if x > bounds.minX && x + sz.width > bounds.maxX {
                y += rowH + lineSpacing
                x = bounds.minX
                rowH = 0
            }
            s.place(at: CGPoint(x: x, y: y), proposal: ProposedViewSize(sz))
            x += sz.width + spacing
            rowH = max(rowH, sz.height)
        }
    }
}

struct Avatar: View {
    let initials: String
    let tone: Int
    var size: CGFloat = 56
    var verified = false
    /// randomuser.me portrait path (e.g. "women/44"). A drawn portrait shows underneath, so there is always a face even offline.
    var portrait: String? = nil
    var circle = false
    /// A local photo (your own) – wins over everything else.
    var image: UIImage? = nil

    var body: some View {
        let radius = circle ? size / 2 : size * 0.36
        ZStack {
            if let image {
                Image(uiImage: image).resizable().scaledToFill()
                    .frame(width: size, height: size).clipped()
            } else if let portrait {
                ZStack {
                    Theme.avatar(tone)
                    FaceArt(key: portrait)
                }
                .frame(width: size, height: size)
                if let url = Person.portraitURL(portrait) {
                    AsyncImage(url: url, transaction: Transaction(animation: .easeOut(duration: 0.25))) { phase in
                        if let photo = phase.image {
                            photo.resizable().scaledToFill()
                        } else {
                            Color.clear
                        }
                    }
                    .frame(width: size, height: size)
                    .clipped()
                }
            } else {
                Text(initials)
                    .font(.k(size * 0.32, .heavy))
                    .foregroundColor(.white)
                    .frame(width: size, height: size)
                    .background(Theme.avatar(tone))
            }
        }
        .frame(width: size, height: size)
        .overlay(
            RoundedRectangle(cornerRadius: radius, style: .continuous)
                .strokeBorder(LinearGradient(colors: [Color.white.opacity(0.55), Color.white.opacity(0)],
                                             startPoint: .topLeading, endPoint: .bottomTrailing),
                              lineWidth: 1.5)
        )
        .clipShape(RoundedRectangle(cornerRadius: radius, style: .continuous))
        .shadow(color: Theme.avatarGlow(tone), radius: 8, y: 4)
        .overlay(alignment: .bottomTrailing) {
            if verified {
                Image(systemName: "checkmark.seal.fill")
                    .font(.system(size: max(15, size * 0.3)))
                    .foregroundColor(Color(hex: 0x10B981))
                    .background(Circle().fill(Color.white).padding(2))
                    .offset(x: 5, y: 5)
                    .accessibilityLabel("Verified")
            }
        }
        .accessibilityElement(children: verified ? .combine : .ignore)
    }
}

/// Vector portrait drawn in SwiftUI, so every member has a face even offline.
/// Seeded from the portrait key ("women/44") so each person always looks the same.
/// The real photo fades in on top when it loads.
struct FaceArt: View {
    let key: String

    private static let skins: [UInt32] = [0xF8DCC6, 0xEFC3A0, 0xD9A27C, 0xBC8059, 0x8F5B3C, 0x6A4129]
    private static let hairs: [UInt32] = [0x2B201C, 0x4B3024, 0x7B4B2A, 0xB5803F, 0x17171B, 0x9096A3]
    private static let tops: [UInt32] = [0x5B50E0, 0xFF7A6B, 0x34D399, 0xF5B301, 0x0EA5E9, 0xEC4899]
    private static let ink = Color(hex: 0x2B2146)

    var body: some View {
        let seed = Person.stableSeed(key)
        let feminine = key.hasPrefix("women")
        let skin = Color(hex: Self.skins[seed % 6])
        let hair = Color(hex: Self.hairs[(seed / 6) % 6])
        let top = Color(hex: Self.tops[(seed / 36) % 6])
        let style = (seed / 5) % 4
        let glasses = seed % 7 == 0
        let beard = !feminine && seed % 3 == 0

        return GeometryReader { g in
            let s = min(g.size.width, g.size.height)
            ZStack {
                hairBack(s, hair, feminine, style)
                // ears
                Circle().fill(skin).frame(width: s * 0.07, height: s * 0.07).position(x: s * 0.3, y: s * 0.49)
                Circle().fill(skin).frame(width: s * 0.07, height: s * 0.07).position(x: s * 0.7, y: s * 0.49)
                // neck + shoulders
                RoundedRectangle(cornerRadius: s * 0.05, style: .continuous).fill(skin)
                    .frame(width: s * 0.19, height: s * 0.22).position(x: s / 2, y: s * 0.66)
                Ellipse().fill(top).frame(width: s * 0.95, height: s * 0.62).position(x: s / 2, y: s * 1.0)
                // head
                Ellipse().fill(skin).frame(width: s * 0.4, height: s * 0.46).position(x: s / 2, y: s * 0.45)
                hairFront(s, hair, feminine, style)
                features(s, glasses: glasses, beard: beard, hair: hair)
            }
            .frame(width: s, height: s)
        }
        .accessibilityHidden(true)
    }

    @ViewBuilder
    private func hairBack(_ s: CGFloat, _ hair: Color, _ feminine: Bool, _ style: Int) -> some View {
        if feminine && style == 0 {
            RoundedRectangle(cornerRadius: s * 0.2, style: .continuous).fill(hair)
                .frame(width: s * 0.5, height: s * 0.62).position(x: s / 2, y: s * 0.58)
        } else if feminine && style == 1 {
            RoundedRectangle(cornerRadius: s * 0.16, style: .continuous).fill(hair)
                .frame(width: s * 0.48, height: s * 0.38).position(x: s / 2, y: s * 0.48)
        } else if feminine && style == 2 {
            Circle().fill(hair).frame(width: s * 0.18, height: s * 0.18).position(x: s / 2, y: s * 0.13)
        }
    }

    @ViewBuilder
    private func hairFront(_ s: CGFloat, _ hair: Color, _ feminine: Bool, _ style: Int) -> some View {
        if (feminine && style == 3) || (!feminine && style == 2) {
            // curls / volume
            let ys: [CGFloat] = [0.36, 0.27, 0.23, 0.22, 0.23, 0.27, 0.36]
            ZStack {
                ForEach(0..<7, id: \.self) { i in
                    Circle().fill(hair).frame(width: s * 0.17, height: s * 0.17)
                        .position(x: s * (0.31 + 0.063 * CGFloat(i)), y: s * ys[i])
                }
                Ellipse().fill(hair).frame(width: s * 0.4, height: s * 0.2).position(x: s / 2, y: s * 0.3)
            }
        } else if !feminine && style == 1 {
            // buzz cut
            Ellipse().fill(hair.opacity(0.9)).frame(width: s * 0.41, height: s * 0.2).position(x: s / 2, y: s * 0.295)
        } else {
            Ellipse().fill(hair).frame(width: s * 0.43, height: s * 0.23).position(x: s / 2, y: s * 0.295)
        }
    }

    private func features(_ s: CGFloat, glasses: Bool, beard: Bool, hair: Color) -> some View {
        let line = max(1, s * 0.014)
        return ZStack {
            if beard {
                Ellipse().fill(hair).frame(width: s * 0.37, height: s * 0.2).position(x: s / 2, y: s * 0.585)
            }
            Circle().fill(Theme.coral.opacity(0.22)).frame(width: s * 0.07, height: s * 0.07)
                .position(x: s * 0.385, y: s * 0.53)
            Circle().fill(Theme.coral.opacity(0.22)).frame(width: s * 0.07, height: s * 0.07)
                .position(x: s * 0.615, y: s * 0.53)
            Circle().fill(Self.ink).frame(width: s * 0.034, height: s * 0.034).position(x: s * 0.42, y: s * 0.475)
            Circle().fill(Self.ink).frame(width: s * 0.034, height: s * 0.034).position(x: s * 0.58, y: s * 0.475)
            Path { p in
                p.move(to: CGPoint(x: s * 0.45, y: s * 0.545))
                p.addQuadCurve(to: CGPoint(x: s * 0.55, y: s * 0.545), control: CGPoint(x: s * 0.5, y: s * 0.585))
            }
            .stroke(beard ? Color.white.opacity(0.85) : Color(hex: 0x6B3A3A),
                    style: StrokeStyle(lineWidth: line, lineCap: .round))
            if glasses {
                Circle().stroke(Self.ink, lineWidth: line).frame(width: s * 0.1, height: s * 0.1)
                    .position(x: s * 0.42, y: s * 0.475)
                Circle().stroke(Self.ink, lineWidth: line).frame(width: s * 0.1, height: s * 0.1)
                    .position(x: s * 0.58, y: s * 0.475)
                Path { p in
                    p.move(to: CGPoint(x: s * 0.47, y: s * 0.475))
                    p.addLine(to: CGPoint(x: s * 0.53, y: s * 0.475))
                }
                .stroke(Self.ink, lineWidth: line)
            }
        }
    }
}

struct RatingBadge: View {
    let summary: RatingSummary

    var body: some View {
        HStack(spacing: 4) {
            Image(systemName: "star.fill").font(.system(size: 10.5, weight: .bold)).foregroundColor(Theme.star)
            Text(summary.label).font(.k(12, .heavy)).foregroundColor(Theme.ink)
            if summary.count > 0 {
                Text("(\(summary.count))").font(.k(11.5, .medium)).foregroundColor(Theme.slate)
            }
        }
        .padding(.horizontal, 9).padding(.vertical, 4)
        .background(Capsule().fill(Theme.star.opacity(0.14)))
        .accessibilityElement(children: .ignore)
        .accessibilityLabel(summary.count == 0 ? "No reviews yet"
                                               : "Rated \(summary.label) from \(summary.count) reviews")
    }
}

struct RatingSummaryView: View {
    let summary: RatingSummary

    var body: some View {
        HStack(spacing: 20) {
            VStack(spacing: 4) {
                Text(summary.label).font(.k(40, .heavy)).foregroundColor(Theme.ink)
                StarRow(rating: Int(summary.average.rounded()), size: 12, spacing: 1)
                Text("\(summary.count) review\(summary.count == 1 ? "" : "s")")
                    .font(.k(11.5, .medium)).foregroundColor(Theme.slate)
            }
            VStack(spacing: 6) {
                ForEach([5, 4, 3, 2, 1], id: \.self) { star in
                    bar(star)
                }
            }
        }
        .accessibilityElement(children: .ignore)
        .accessibilityLabel("Average \(summary.label) out of 5 from \(summary.count) reviews")
    }

    private func bar(_ star: Int) -> some View {
        let n = summary.histogram[star - 1]
        let fraction = CGFloat(n) / CGFloat(max(summary.count, 1))
        return HStack(spacing: 8) {
            Text("\(star)").font(.k(11, .bold)).foregroundColor(Theme.slate).frame(width: 10)
            GeometryReader { g in
                ZStack(alignment: .leading) {
                    Capsule().fill(Theme.chipBg)
                    Capsule().fill(Theme.star).frame(width: g.size.width * fraction)
                }
            }
            .frame(height: 6)
        }
    }
}

struct ReviewCard: View {
    let review: Review
    var inPerson = true
    var plain = false                      // soft panel instead of a floating card
    var helpful = false
    var onHelpful: (() -> Void)? = nil
    @EnvironmentObject private var store: Store

    var body: some View {
        if plain {
            content.background(RoundedRectangle(cornerRadius: 16, style: .continuous).fill(Theme.canvas))
        } else {
            content.cardChrome(radius: 18)
        }
    }

    private var content: some View {
        VStack(alignment: .leading, spacing: 10) {
            HStack(spacing: 10) {
                if review.isYou {
                    Avatar(initials: "YO", tone: 0, size: 38, circle: true, image: store.myPhoto)
                } else {
                    Avatar(initials: review.author.initials, tone: review.author.count % 4, size: 38,
                           portrait: review.portrait, circle: true)
                }
                VStack(alignment: .leading, spacing: 3) {
                    Text(review.author).font(.k(14, .heavy)).foregroundColor(Theme.ink)
                    HStack(spacing: 6) {
                        StarRow(rating: review.rating, size: 11, spacing: 1)
                        Text("· \(review.when)").font(.k(11.5, .medium)).foregroundColor(Theme.muted)
                    }
                }
                Spacer(minLength: 4)
            }
            if !review.text.isEmpty {
                Text(review.text)
                    .font(.k(13.5)).foregroundColor(Theme.slateDark)
                    .lineSpacing(3).fixedSize(horizontal: false, vertical: true)
            }
            HStack(spacing: 8) {
                HStack(spacing: 5) {
                    Image(systemName: inPerson ? "person.2.fill" : "video.fill")
                    Text(review.context)
                }
                .font(.k(11, .bold)).foregroundColor(Theme.inkLight)
                .padding(.horizontal, 9).padding(.vertical, 4)
                .background(Capsule().fill(Theme.lavender))
                Spacer()
                if let onHelpful {
                    let count = review.helpful + (helpful ? 1 : 0)
                    Button(action: onHelpful) {
                        HStack(spacing: 5) {
                            Image(systemName: helpful ? "hand.thumbsup.fill" : "hand.thumbsup")
                            Text(count > 0 ? "Helpful · \(count)" : "Helpful")
                        }
                        .font(.k(11.5, .bold))
                        .foregroundColor(helpful ? Theme.mintDark : Theme.slate)
                    }
                    .buttonStyle(PressStyle())
                    .accessibilityLabel(helpful ? "Remove helpful mark" : "Mark review as helpful")
                    .accessibilityValue("\(count) found this helpful")
                }
            }
        }
        .padding(14)
    }
}

/// A remote photo with a spinner while loading and a graceful fallback if it can't be fetched.
struct RemotePhoto: View {
    let url: URL
    var tone = 0
    var fit = false

    var body: some View {
        AsyncImage(url: url, transaction: Transaction(animation: .easeOut(duration: 0.3))) { phase in
            switch phase {
            case .success(let image):
                if fit { image.resizable().scaledToFit() }
                else { image.resizable().scaledToFill() }
            case .failure:
                ZStack {
                    Theme.avatar(tone)
                    Image(systemName: "photo").font(.system(size: 26)).foregroundColor(.white.opacity(0.75))
                }
            default:
                ZStack { Theme.chipBg; ProgressView() }
            }
        }
    }
}

struct PhotoStart: Identifiable { let id: Int }

/// Full-screen, swipeable photo viewer.
struct PhotoViewer: View {
    let urls: [URL]
    let title: String
    @Environment(\.dismiss) private var dismiss
    @State private var index: Int

    init(urls: [URL], start: Int, title: String) {
        self.urls = urls
        self.title = title
        _index = State(initialValue: start)
    }

    var body: some View {
        ZStack(alignment: .top) {
            Color.black.ignoresSafeArea()
            TabView(selection: $index) {
                ForEach(Array(urls.enumerated()), id: \.offset) { i, url in
                    RemotePhoto(url: url, fit: true)
                        .tag(i)
                        .accessibilityLabel("Photo \(i + 1) of \(urls.count)")
                }
            }
            .tabViewStyle(.page(indexDisplayMode: .always))
            .ignoresSafeArea(edges: .bottom)

            HStack {
                Text("\(title)'s photos")
                    .font(.k(15, .heavy)).foregroundColor(.white)
                Spacer()
                Button { dismiss() } label: {
                    Image(systemName: "xmark")
                        .font(.system(size: 14, weight: .bold)).foregroundColor(.white)
                        .frame(width: 34, height: 34)
                        .background(Circle().fill(Color.white.opacity(0.18)))
                }
                .accessibilityLabel("Close photos")
            }
            .padding(.horizontal, 18).padding(.top, 10)
        }
        .preferredColorScheme(.dark)
    }
}

struct ActionButton: View {
    enum Kind { case coral, mint, lavender }
    let title: String
    var icon: String? = nil
    var kind: Kind = .coral
    var full = false

    private var fg: Color { kind == .coral ? .white : (kind == .mint ? Theme.mintDark : Theme.ink) }

    @ViewBuilder private var fill: some View {
        switch kind {
        case .coral: Theme.coralGradient
        case .mint: Theme.mint.opacity(0.16)
        case .lavender: Theme.lavender
        }
    }

    var body: some View {
        HStack(spacing: 6) {
            if let icon { Image(systemName: icon).font(.system(size: full ? 14 : 12, weight: .bold)) }
            Text(title)
        }
        .font(.k(full ? 16 : 13, .bold))
        .foregroundColor(fg)
        .padding(.horizontal, full ? 18 : 15)
        .padding(.vertical, full ? 16 : 10)
        .frame(maxWidth: full ? .infinity : nil)
        .background(fill)
        .clipShape(RoundedRectangle(cornerRadius: full ? 18 : 22, style: .continuous))
        .shadow(color: kind == .coral ? Theme.coral.opacity(0.35) : .clear, radius: 10, y: 5)
    }
}

struct Chip: View {
    let text: String
    var icon: String? = nil
    var on = false

    var body: some View {
        HStack(spacing: 6) {
            if let icon { Image(systemName: icon).font(.system(size: 11.5, weight: .bold)) }
            Text(text)
        }
        .font(.k(13, on ? .bold : .semibold))
        .foregroundColor(on ? .white : Theme.slateDark)
        .padding(.horizontal, 13).padding(.vertical, 8)
        .background(
            ZStack {
                Capsule().fill(Color.white)
                Capsule().fill(Theme.brand).opacity(on ? 1 : 0)
            }
        )
        .overlay(Capsule().strokeBorder(on ? Color.clear : Theme.line, lineWidth: 1))
        .shadow(color: Theme.ink.opacity(on ? 0.25 : 0), radius: 6, y: 3)
        .accessibilityElement(children: .combine)
        .accessibilityAddTraits(on ? .isSelected : [])
    }
}

struct SectionLabel: View {
    let text: String
    var body: some View {
        Text(text.uppercased())
            .font(.k(10.5, .heavy)).kerning(1.1)
            .foregroundColor(Theme.muted)
            .accessibilityAddTraits(.isHeader)
    }
}

struct StarRow: View {
    let rating: Int
    var size: CGFloat = 14
    var spacing: CGFloat = 2
    var onSelect: ((Int) -> Void)? = nil   // pass a closure to make the stars tappable

    var body: some View {
        HStack(spacing: spacing) {
            ForEach(1...5, id: \.self) { i in
                if let onSelect {
                    Button { onSelect(i) } label: { star(i) }
                        .buttonStyle(PressStyle())
                        .accessibilityLabel("\(i) star\(i == 1 ? "" : "s")")
                        .accessibilityAddTraits(i == rating ? .isSelected : [])
                } else {
                    star(i)
                }
            }
        }
        .font(.system(size: size))
        .accessibilityElement(children: onSelect == nil ? .ignore : .contain)
        .accessibilityLabel(onSelect == nil ? "Rated \(rating) out of 5" : "Rating")
    }

    private func star(_ i: Int) -> some View {
        Image(systemName: i <= rating ? "star.fill" : "star")
            .foregroundColor(i <= rating ? Theme.star : Color(hex: 0xCFD5E3))
            .scaleEffect(i <= rating ? 1 : 0.92)
    }
}

struct MatchRing: View {
    let value: Int
    var size: CGFloat = 48
    @State private var shown = false
    @Environment(\.accessibilityReduceMotion) private var reduceMotion

    var body: some View {
        ZStack {
            Circle().stroke(Theme.lavender, lineWidth: 4.5)
            Circle()
                .trim(from: 0, to: shown ? CGFloat(value) / 100 : 0)
                .stroke(Theme.brand, style: StrokeStyle(lineWidth: 4.5, lineCap: .round))
                .rotationEffect(.degrees(-90))
                .animation(reduceMotion ? nil : .easeOut(duration: 0.5), value: value)
            VStack(spacing: -1) {
                Text("\(value)").font(.k(size * 0.32, .heavy)).foregroundColor(Theme.ink)
                    .contentTransition(.numericText())
                Text("match").font(.k(size * 0.15, .bold)).foregroundColor(Theme.muted)
            }
        }
        .frame(width: size, height: size)
        .onAppear {
            if reduceMotion { shown = true }
            else { withAnimation(.easeOut(duration: 0.9).delay(0.1)) { shown = true } }
        }
        .accessibilityElement(children: .ignore)
        .accessibilityLabel("\(value) percent match")
    }
}

struct BrandHeader: View {
    var body: some View {
        HStack {
            HStack(spacing: 9) {
                Image(systemName: "sparkles")
                    .font(.system(size: 15, weight: .bold)).foregroundColor(.white)
                    .frame(width: 34, height: 34).background(Theme.brand)
                    .clipShape(RoundedRectangle(cornerRadius: 11, style: .continuous))
                    .shadow(color: Theme.ink.opacity(0.35), radius: 6, y: 4)
                    .accessibilityHidden(true)
                Text("Kindred").font(.k(20, .heavy)).foregroundColor(Theme.ink)
            }
            Spacer()
            HStack(spacing: 5) {
                Image(systemName: "mappin.and.ellipse").foregroundColor(Theme.coral)
                Text(Config.homeCity)
            }
            .font(.k(12.5, .bold)).foregroundColor(Theme.slateDark)
            .padding(.horizontal, 12).padding(.vertical, 7)
            .background(Capsule().fill(Color.white))
            .overlay(Capsule().strokeBorder(Theme.line, lineWidth: 1))
            .shadow(color: Theme.ink.opacity(0.06), radius: 6, y: 3)
            .accessibilityElement(children: .combine)
            .accessibilityLabel("Your city: \(Config.homeCity)")
        }
    }
}

struct ScreenTitle: View {
    let title: String
    let subtitle: String
    var body: some View {
        VStack(alignment: .leading, spacing: 3) {
            Text(title).font(.k(28, .heavy)).foregroundColor(Theme.ink)
                .accessibilityAddTraits(.isHeader)
            Text(subtitle).font(.k(14, .medium)).foregroundColor(Theme.slate)
        }
    }
}

// MARK: - Root + bottom navigation

struct RootView: View {
    @StateObject private var store = Store()

    init() {
        let a = UITabBarAppearance()
        a.configureWithDefaultBackground()
        UITabBar.appearance().standardAppearance = a
        UITabBar.appearance().scrollEdgeAppearance = a
    }

    var body: some View {
        TabView {
            DiscoverView().tabItem { Label("Discover", systemImage: "safari") }
            EventsView().tabItem { Label("Events", systemImage: "calendar") }
            ConnectionsView().tabItem { Label("Connections", systemImage: "person.2") }
                .badge(store.pendingReviews)
            ProfileView().tabItem { Label("Profile", systemImage: "person") }
        }
        .tint(Theme.ink)
        .environmentObject(store)
        .preferredColorScheme(.light)
    }
}

// MARK: - Discover

enum DiscoverScope { case local, global }

struct ScopeToggle: View {
    @Binding var scope: DiscoverScope
    let localCount: Int
    let globalCount: Int
    @Namespace private var pill

    var body: some View {
        HStack(spacing: 0) {
            segment(.local, "Local City", "mappin.and.ellipse", localCount)
            segment(.global, "Global", "globe", globalCount)
        }
        .padding(4)
        .background(Theme.chipBg)
        .clipShape(RoundedRectangle(cornerRadius: 16, style: .continuous))
    }

    private func segment(_ value: DiscoverScope, _ title: String, _ icon: String, _ count: Int) -> some View {
        let on = scope == value
        return Button {
            Haptics.tap()
            withAnimation(.spring(response: 0.35, dampingFraction: 0.8)) { scope = value }
        } label: {
            HStack(spacing: 6) {
                Image(systemName: icon)
                Text(title)
                Text("\(count)").font(.k(11, .bold))
                    .padding(.horizontal, 7).padding(.vertical, 1)
                    .background(Capsule().fill(on ? Theme.lavender : Theme.line))
            }
            .font(.k(13, .bold))
            .foregroundColor(on ? Theme.ink : Theme.slate)
            .frame(maxWidth: .infinity).padding(.vertical, 10)
            .background {
                if on {
                    RoundedRectangle(cornerRadius: 12, style: .continuous)
                        .fill(Color.white)
                        .shadow(color: Theme.ink.opacity(0.10), radius: 5, y: 2)
                        .matchedGeometryEffect(id: "pill", in: pill)
                }
            }
            .contentShape(Rectangle())
        }
        .buttonStyle(.plain)
        .accessibilityLabel("\(title), \(count) people")
        .accessibilityAddTraits(on ? .isSelected : [])
    }
}

struct HeroBanner: View {
    let count: Int
    let people: [Person]

    var body: some View {
        VStack(alignment: .leading, spacing: 12) {
            HStack(spacing: 6) {
                Image(systemName: "checkmark.shield.fill")
                Text("Every member is Face ID verified")
            }
            .font(.k(11, .bold)).foregroundColor(.white.opacity(0.92))
            .padding(.horizontal, 10).padding(.vertical, 5)
            .background(Capsule().fill(Color.white.opacity(0.16)))

            Text("Friends who actually\nget you")
                .font(.k(25, .heavy)).foregroundColor(.white)
                .lineSpacing(1)

            HStack(spacing: 10) {
                if !people.isEmpty {
                    HStack(spacing: -9) {
                        ForEach(people) { p in
                            Avatar(initials: p.initials, tone: p.tone, size: 28, portrait: p.portrait, circle: true)
                                .overlay(Circle().stroke(Theme.inkLight, lineWidth: 2))
                        }
                    }
                    .accessibilityHidden(true)
                }
                Text(count > 0 ? "\(count) \(count == 1 ? "person" : "people") in \(Config.homeCity) share your interests"
                               : "Add interests on your profile to find your people")
                    .font(.k(12.5, .medium)).foregroundColor(Theme.lavender.opacity(0.88))
                    .fixedSize(horizontal: false, vertical: true)
            }
        }
        .frame(maxWidth: .infinity, alignment: .leading)
        .padding(20)
        .background(
            LinearGradient(colors: [Theme.ink, Theme.inkLight], startPoint: .topLeading, endPoint: .bottomTrailing)
                .overlay(alignment: .topTrailing) {
                    Circle().fill(Color.white.opacity(0.10)).frame(width: 150, height: 150).offset(x: 40, y: -50)
                }
                .overlay(alignment: .bottomTrailing) {
                    Circle().fill(Theme.coral.opacity(0.35)).frame(width: 90, height: 90)
                        .blur(radius: 18).offset(x: -10, y: 30)
                }
        )
        .clipShape(RoundedRectangle(cornerRadius: 26, style: .continuous))
        .shadow(color: Theme.ink.opacity(0.30), radius: 18, y: 10)
    }
}

struct TagPill: View {
    let text: String
    let shared: Bool

    var body: some View {
        HStack(spacing: 5) {
            Image(systemName: Person.icon(for: text)).font(.system(size: 10, weight: .bold))
            Text(text)
        }
        .font(.k(11.5, shared ? .bold : .medium))
        .foregroundColor(shared ? Theme.ink : Theme.slateDark)
        .padding(.horizontal, 10).padding(.vertical, 5)
        .background(Capsule().fill(shared ? Theme.lavender : Theme.chipBg))
        .accessibilityElement(children: .combine)
        .accessibilityLabel(shared ? "\(text), shared interest" : text)
    }
}

struct PersonalityPill: View {
    let code: String
    let type: String
    var body: some View {
        HStack(spacing: 5) {
            Image(systemName: Person.typeIcon(for: type)).foregroundColor(Theme.inkLight)
            Text("\(code) · \(type)")
        }
        .font(.k(11.5, .bold)).foregroundColor(Theme.slateDark)
        .padding(.horizontal, 10).padding(.vertical, 4)
        .background(Capsule().fill(Theme.chipBg))
        .accessibilityElement(children: .combine)
    }
}

struct ProfileCard: View {
    let person: Person
    let requested: Bool
    let mine: Set<String>
    let reviews: [Review]
    let onRequest: () -> Void
    let onOpen: () -> Void

    var body: some View {
        let common = person.shared(with: mine)
        VStack(alignment: .leading, spacing: 14) {
            HStack(alignment: .center, spacing: 14) {
                Avatar(initials: person.initials, tone: person.tone, size: 60, verified: true, portrait: person.portrait)
                VStack(alignment: .leading, spacing: 5) {
                    Text("\(person.name), \(person.age)")
                        .font(.k(17, .heavy)).foregroundColor(Theme.ink).lineLimit(1)
                    HStack(spacing: 4) {
                        Image(systemName: "mappin.and.ellipse").foregroundColor(Theme.coral)
                        Text("\(person.city) · \(person.distance)")
                    }
                    .font(.k(12, .medium)).foregroundColor(Theme.slate)
                    PersonalityPill(code: person.code, type: person.type)
                }
                Spacer(minLength: 4)
                MatchRing(value: person.score(with: mine))
            }

            Text(person.bio)
                .font(.k(14)).foregroundColor(Theme.slateDark)
                .lineSpacing(3).lineLimit(3)
                .fixedSize(horizontal: false, vertical: true)

            FlowLayout(spacing: 6, lineSpacing: 6) {
                ForEach(person.tags, id: \.self) { TagPill(text: $0, shared: mine.contains($0)) }
            }

            if let top = reviews.filter({ !$0.isYou && !$0.text.isEmpty }).max(by: { $0.helpful < $1.helpful }) {
                HStack(alignment: .top, spacing: 8) {
                    Image(systemName: "quote.opening")
                        .font(.system(size: 11, weight: .bold)).foregroundColor(Theme.coral).padding(.top, 3)
                    VStack(alignment: .leading, spacing: 3) {
                        Text(top.text)
                            .font(.k(12.5)).foregroundColor(Theme.slateDark)
                            .lineSpacing(2).lineLimit(2)
                        Text("\(top.authorFirst) · \(top.context)")
                            .font(.k(11, .semibold)).foregroundColor(Theme.muted)
                    }
                }
                .padding(12)
                .frame(maxWidth: .infinity, alignment: .leading)
                .background(RoundedRectangle(cornerRadius: 14, style: .continuous).fill(Theme.chipBg))
                .accessibilityElement(children: .combine)
                .accessibilityLabel("Top review: \(top.text)")
            }

            HairLine()

            HStack {
                HStack(spacing: 10) {
                    HStack(spacing: 6) {
                        Image(systemName: common > 0 ? "heart.fill" : "heart")
                            .foregroundColor(common > 0 ? Theme.coral : Theme.muted)
                        Text("\(common) in common")
                    }
                    .font(.k(12.5, .semibold)).foregroundColor(Theme.slate)
                    RatingBadge(summary: RatingSummary(reviews))
                }
                Spacer(minLength: 6)
                Button(action: onRequest) {
                    if requested {
                        ActionButton(title: "Requested", icon: "checkmark", kind: .mint)
                    } else {
                        ActionButton(title: "Send request", icon: "person.badge.plus")
                    }
                }
                .buttonStyle(PressStyle())
                .accessibilityLabel(requested ? "Cancel request to \(person.name)" : "Send request to \(person.name)")
            }
        }
        .card(padding: 16)
        .contentShape(Rectangle())
        .onTapGesture(perform: onOpen)
        .accessibilityAction(named: "View profile", onOpen)
    }
}

struct PersonSheet: View {
    let person: Person
    @EnvironmentObject private var store: Store
    @State private var viewer: PhotoStart?
    @State private var showAllReviews = false

    private var reviews: [Review] { store.profileReviews(for: person) }

    var body: some View {
        let photos = person.photoURLs
        let reviews = self.reviews
        let summary = RatingSummary(reviews)
        ScrollView(showsIndicators: false) {
            VStack(spacing: 0) {
                hero
                VStack(spacing: 20) {
                    identity
                    statsRow(summary: summary)
                    aboutSection
                    photosSection(photos)
                    interestsSection
                    reviewsSection(reviews, summary: summary)
                }
                .padding(.horizontal, 22).padding(.top, 56).padding(.bottom, 24)
            }
        }
        .background(Theme.canvas.ignoresSafeArea())
        .safeAreaInset(edge: .bottom, spacing: 0) { actionBar }
        .fullScreenCover(item: $viewer) { start in
            PhotoViewer(urls: photos, start: start.id, title: person.firstName)
        }
    }

    // MARK: Pieces

    private var hero: some View {
        ZStack(alignment: .bottom) {
            ZStack {
                Theme.brand
                Circle().fill(Color.white.opacity(0.10)).frame(width: 190, height: 190).offset(x: 120, y: -30)
                Circle().fill(Theme.coral.opacity(0.35)).frame(width: 110, height: 110)
                    .blur(radius: 26).offset(x: -110, y: 30)
            }
            .frame(height: 124).clipped()

            Avatar(initials: person.initials, tone: person.tone, size: 104, verified: true, portrait: person.portrait)
                .padding(4)
                .background(RoundedRectangle(cornerRadius: 104 * 0.36 + 4, style: .continuous).fill(Color.white))
                .shadow(color: Theme.ink.opacity(0.18), radius: 12, y: 6)
                .offset(y: 52)
        }
        .padding(.bottom, 0)
    }

    private var identity: some View {
        VStack(spacing: 8) {
            Text("\(person.name), \(person.age)")
                .font(.k(25, .heavy)).foregroundColor(Theme.ink)
            HStack(spacing: 4) {
                Image(systemName: "mappin.and.ellipse").foregroundColor(Theme.coral)
                Text("\(person.city) · \(person.distance)")
            }
            .font(.k(13, .medium)).foregroundColor(Theme.slate)
            PersonalityPill(code: person.code, type: person.type)
        }
    }

    private func statsRow(summary: RatingSummary) -> some View {
        let common = person.shared(with: store.myInterests)
        return HStack(spacing: 0) {
            statCell("\(person.score(with: store.myInterests))%", "Match")
            Rectangle().fill(Theme.line).frame(width: 1, height: 30)
            statCell(summary.label, "Rating", star: summary.count > 0)
            Rectangle().fill(Theme.line).frame(width: 1, height: 30)
            statCell("\(common)/\(person.tags.count)", "In common")
        }
        .padding(.vertical, 14)
        .cardChrome(radius: 20)
    }

    private func statCell(_ value: String, _ label: String, star: Bool = false) -> some View {
        VStack(spacing: 2) {
            HStack(spacing: 4) {
                Text(value).font(.k(20, .heavy)).foregroundColor(Theme.ink)
                if star { Image(systemName: "star.fill").font(.system(size: 13)).foregroundColor(Theme.star) }
            }
            Text(label).font(.k(11.5, .medium)).foregroundColor(Theme.slate)
        }
        .frame(maxWidth: .infinity)
        .accessibilityElement(children: .combine)
    }

    private var aboutSection: some View {
        VStack(alignment: .leading, spacing: 8) {
            SectionLabel(text: "About \(person.firstName)")
            Text(person.bio)
                .font(.k(14.5)).foregroundColor(Theme.slateDark)
                .lineSpacing(4).fixedSize(horizontal: false, vertical: true)
        }
        .frame(maxWidth: .infinity, alignment: .leading)
    }

    // Photo gallery – bleeds to the screen edges, tap to open full screen
    private func photosSection(_ photos: [URL]) -> some View {
        VStack(alignment: .leading, spacing: 8) {
            SectionLabel(text: "Photos").padding(.horizontal, 22)
            ScrollView(.horizontal, showsIndicators: false) {
                HStack(spacing: 10) {
                    ForEach(Array(photos.enumerated()), id: \.offset) { i, url in
                        Button { viewer = PhotoStart(id: i) } label: {
                            RemotePhoto(url: url, tone: person.tone)
                                .frame(width: 132, height: 168)
                                .background(Theme.chipBg)
                                .clipShape(RoundedRectangle(cornerRadius: 18, style: .continuous))
                                .overlay(RoundedRectangle(cornerRadius: 18, style: .continuous)
                                    .strokeBorder(Color.white.opacity(0.7), lineWidth: 1))
                                .shadow(color: Theme.ink.opacity(0.14), radius: 8, y: 4)
                        }
                        .buttonStyle(PressStyle())
                        .accessibilityLabel("Photo \(i + 1) of \(photos.count) from \(person.firstName)")
                        .accessibilityHint("Opens full screen")
                    }
                }
                .padding(.horizontal, 22).padding(.vertical, 8)
            }
        }
        .padding(.horizontal, -22)
    }

    private var interestsSection: some View {
        let common = person.shared(with: store.myInterests)
        return VStack(alignment: .leading, spacing: 8) {
            SectionLabel(text: common > 0 ? "\(common) interests in common" : "Interests")
            FlowLayout(spacing: 6, lineSpacing: 6) {
                ForEach(person.tags, id: \.self) {
                    TagPill(text: $0, shared: store.myInterests.contains($0))
                }
            }
        }
        .frame(maxWidth: .infinity, alignment: .leading)
    }

    private func reviewsSection(_ reviews: [Review], summary: RatingSummary) -> some View {
        let visible = showAllReviews ? reviews : Array(reviews.prefix(3))
        return VStack(alignment: .leading, spacing: 12) {
            HStack {
                SectionLabel(text: "Reviews")
                Spacer()
                if reviews.count > 3 {
                    Button {
                        Haptics.tap()
                        withAnimation(.spring(response: 0.4, dampingFraction: 0.85)) { showAllReviews.toggle() }
                    } label: {
                        Text(showAllReviews ? "Show less" : "See all \(reviews.count)")
                            .font(.k(12.5, .bold)).foregroundColor(Theme.inkLight)
                    }
                }
            }
            if reviews.isEmpty {
                VStack(spacing: 6) {
                    Image(systemName: "text.bubble").font(.system(size: 26)).foregroundColor(Theme.lavenderMid)
                    Text("No reviews yet").font(.k(15, .heavy)).foregroundColor(Theme.ink)
                    Text("Meet \(person.firstName) and be the first to share how it went.")
                        .font(.k(12.5)).foregroundColor(Theme.slate).multilineTextAlignment(.center)
                }
                .frame(maxWidth: .infinity).card(padding: 22)
            } else {
                RatingSummaryView(summary: summary).card(padding: 18)
                ForEach(visible) { r in
                    ReviewCard(review: r, inPerson: person.isLocal, helpful: store.isHelpful(r.id),
                               onHelpful: r.isYou ? nil : helpfulAction(r.id))
                }
            }
        }
        .frame(maxWidth: .infinity, alignment: .leading)
    }

    private func helpfulAction(_ id: String) -> () -> Void {
        { Haptics.tap(); withAnimation(.easeInOut(duration: 0.2)) { store.toggleHelpful(id) } }
    }

    /// Always-visible call to action, so you never have to scroll back up to send a request.
    private var actionBar: some View {
        let requested = store.requested.contains(person.id)
        return Button {
            Haptics.success()
            withAnimation(.easeInOut(duration: 0.2)) { store.toggleRequest(person) }
        } label: {
            ActionButton(title: requested ? "Cancel request" : "Send request to \(person.firstName)",
                         icon: requested ? "xmark" : "person.badge.plus",
                         kind: requested ? .lavender : .coral, full: true)
        }
        .buttonStyle(PressStyle())
        .padding(.horizontal, 22).padding(.vertical, 12)
        .frame(maxWidth: .infinity)
        .background(.ultraThinMaterial)
        .overlay(alignment: .top) { HairLine() }
    }
}

struct DiscoverView: View {
    @EnvironmentObject private var store: Store
    @State private var scope: DiscoverScope = .local
    @State private var selectedInterests: Set<String> = []
    @State private var selectedType: String? = nil
    @State private var detail: Person?

    private var hasFilters: Bool { !selectedInterests.isEmpty || selectedType != nil }

    private func filteredPeople(mine: Set<String>) -> [Person] {
        Person.samples
            .filter { scope == .global || $0.isLocal }
            .filter { selectedInterests.isEmpty || !selectedInterests.isDisjoint(with: $0.tags) }
            .filter { selectedType == nil || $0.type == selectedType }
            .map { (person: $0, score: $0.score(with: mine)) }
            .sorted { $0.score == $1.score ? $0.person.name < $1.person.name : $0.score > $1.score }
            .map(\.person)
    }

    var body: some View {
        let mine = store.myInterests
        let people = filteredPeople(mine: mine)
        let sharedPeople = Person.samples
            .filter { $0.isLocal && $0.shared(with: mine) > 0 }
            .sorted { $0.score(with: mine) > $1.score(with: mine) }

        ScrollView(showsIndicators: false) {
            VStack(alignment: .leading, spacing: 18) {
                HeroBanner(count: sharedPeople.count, people: Array(sharedPeople.prefix(3)))
                    .padding(.horizontal, 16)

                chipRow("Interests", items: Person.allInterests,
                        icon: { Person.icon(for: $0) },
                        isOn: { selectedInterests.contains($0) },
                        toggle: { tag in
                            if selectedInterests.contains(tag) { selectedInterests.remove(tag) }
                            else { selectedInterests.insert(tag) }
                        })
                chipRow("Personality", items: Person.allTypes,
                        icon: { Person.typeIcon(for: $0) },
                        isOn: { selectedType == $0 },
                        toggle: { selectedType = (selectedType == $0) ? nil : $0 })

                HStack {
                    Text("\(people.count) \(people.count == 1 ? "person" : "people") \(scope == .local ? "in \(Config.homeCity)" : "worldwide")")
                        .font(.k(14, .heavy)).foregroundColor(Theme.ink)
                    Text("· best match first")
                        .font(.k(12.5, .medium)).foregroundColor(Theme.muted)
                    Spacer()
                    Button { clearFilters() } label: {
                        Text("Clear filters").font(.k(12.5, .bold)).foregroundColor(Theme.inkLight)
                    }
                    .opacity(hasFilters ? 1 : 0)
                    .disabled(!hasFilters)
                }
                .padding(.horizontal, 18)

                if people.isEmpty {
                    emptyState.padding(.horizontal, 16)
                } else {
                    LazyVStack(spacing: 16) {
                        ForEach(people) { person in
                            ProfileCard(person: person,
                                        requested: store.requested.contains(person.id),
                                        mine: mine,
                                        reviews: store.profileReviews(for: person),
                                        onRequest: {
                                            Haptics.success()
                                            withAnimation(.easeInOut(duration: 0.2)) { store.toggleRequest(person) }
                                        },
                                        onOpen: { detail = person })
                                .transition(.opacity.combined(with: .scale(scale: 0.97)))
                        }
                    }
                    .padding(.horizontal, 16)
                }
            }
            .padding(.top, 10).padding(.bottom, 28)
            .animation(.spring(response: 0.4, dampingFraction: 0.85), value: people.map(\.id))
        }
        .background(AmbientBackground())
        .safeAreaInset(edge: .top, spacing: 0) {
            VStack(spacing: 12) {
                BrandHeader()
                ScopeToggle(scope: $scope,
                            localCount: Person.samples.filter { $0.isLocal }.count,
                            globalCount: Person.samples.count)
            }
            .padding(.horizontal, 16).padding(.top, 6).padding(.bottom, 12)
            .background(.ultraThinMaterial)
        }
        .sheet(item: $detail) { p in
            PersonSheet(person: p)
                .presentationDetents([.large])
                .presentationDragIndicator(.visible)
        }
    }

    private var emptyState: some View {
        VStack(spacing: 10) {
            Image(systemName: "person.crop.circle.badge.questionmark")
                .font(.system(size: 40)).foregroundColor(Theme.lavenderMid)
            Text("No one matches those filters yet")
                .font(.k(16, .heavy)).foregroundColor(Theme.ink)
            Text(scope == .local ? "Try removing a filter or switching to Global."
                                 : "Try removing a filter.")
                .font(.k(13)).foregroundColor(Theme.slate)
            Button { clearFilters() } label: { ActionButton(title: "Clear filters", kind: .lavender) }
                .buttonStyle(PressStyle()).padding(.top, 4)
        }
        .frame(maxWidth: .infinity).card(padding: 28)
    }

    private func clearFilters() {
        Haptics.tap()
        withAnimation(.easeInOut(duration: 0.2)) {
            selectedInterests = []
            selectedType = nil
        }
    }

    private func chipRow(_ title: String, items: [String],
                         icon: @escaping (String) -> String,
                         isOn: @escaping (String) -> Bool,
                         toggle: @escaping (String) -> Void) -> some View {
        VStack(alignment: .leading, spacing: 9) {
            SectionLabel(text: title).padding(.horizontal, 18)
            ScrollView(.horizontal, showsIndicators: false) {
                HStack(spacing: 8) {
                    ForEach(items, id: \.self) { item in
                        Button {
                            Haptics.tap()
                            withAnimation(.easeInOut(duration: 0.2)) { toggle(item) }
                        } label: { Chip(text: item, icon: icon(item), on: isOn(item)) }
                        .buttonStyle(PressStyle())
                    }
                }
                .padding(.horizontal, 16).padding(.vertical, 8)   // room for chip shadows
            }
            .padding(.vertical, -8)
        }
    }
}

// MARK: - Events

struct FeaturedEvent: View {
    let event: EventItem
    let onVote: () -> Void

    private var cover: some View {
        LinearGradient(colors: [Theme.ink, Color(hex: 0x6D5BD0), Theme.coral],
                       startPoint: .topLeading, endPoint: .bottomTrailing)
            .overlay(alignment: .topTrailing) {
                Circle().fill(Color.white.opacity(0.12)).frame(width: 170, height: 170).offset(x: 50, y: -60)
            }
            .overlay(alignment: .bottomLeading) {
                Circle().fill(Color.white.opacity(0.08)).frame(width: 110, height: 110).offset(x: -30, y: 40)
            }
    }

    var body: some View {
        VStack(spacing: 0) {
            ZStack(alignment: .topLeading) {
                cover
                VStack(alignment: .leading, spacing: 0) {
                    HStack(alignment: .top) {
                        VStack(spacing: 0) {
                            Text(event.month).font(.k(10, .heavy)).kerning(1).foregroundColor(Theme.coral)
                            Text(event.day).font(.k(20, .heavy)).foregroundColor(Theme.ink)
                        }
                        .padding(.horizontal, 12).padding(.vertical, 6)
                        .background(RoundedRectangle(cornerRadius: 13, style: .continuous).fill(Color.white))
                        .shadow(color: .black.opacity(0.15), radius: 6, y: 3)
                        .accessibilityElement(children: .combine)
                        Spacer()
                        Text("🔥 Top voted")
                            .font(.k(11, .bold)).foregroundColor(.white)
                            .padding(.horizontal, 10).padding(.vertical, 5)
                            .background(Capsule().fill(Color.white.opacity(0.22)))
                    }
                    Spacer()
                    Text(event.title).font(.k(21, .heavy)).foregroundColor(.white)
                    HStack(spacing: 4) {
                        Image(systemName: "mappin.and.ellipse")
                        Text(event.place)
                    }
                    .font(.k(12.5, .semibold)).foregroundColor(.white.opacity(0.85))
                    .padding(.top, 2)
                }
                .padding(14)
            }
            .frame(height: 168).clipped()

            HStack {
                HStack(spacing: -8) {
                    ForEach(Array(Person.samples.filter(\.isLocal).prefix(4))) { p in
                        Avatar(initials: p.initials, tone: p.tone, size: 28, portrait: p.portrait, circle: true)
                            .overlay(Circle().stroke(Color.white, lineWidth: 2))
                    }
                }
                .accessibilityHidden(true)
                Text("\(event.going) going").font(.k(12.5, .semibold)).foregroundColor(Theme.slate).padding(.leading, 12)
                Spacer()
                Button(action: onVote) {
                    ActionButton(title: "\(event.votes) votes", icon: "arrow.up",
                                 kind: event.voted ? .mint : .lavender)
                        .contentTransition(.numericText())
                }
                .buttonStyle(PressStyle())
                .accessibilityLabel(event.voted ? "Remove your vote" : "Vote for \(event.title)")
                .accessibilityValue("\(event.votes) votes")
            }
            .padding(14)
        }
        .cardChrome(radius: 26)
    }
}

struct EventRow: View {
    let event: EventItem
    let fraction: Double
    let onVote: () -> Void

    var body: some View {
        HStack(spacing: 12) {
            VStack(spacing: 2) {
                Text(event.day).font(.k(20, .heavy))
                Text(event.month).font(.k(9.5, .heavy)).kerning(1).foregroundColor(Theme.inkLight)
            }
            .foregroundColor(Theme.ink)
            .frame(width: 54, height: 58)
            .background(RoundedRectangle(cornerRadius: 16, style: .continuous).fill(Theme.lavender))
            .accessibilityElement(children: .combine)

            VStack(alignment: .leading, spacing: 4) {
                Text(event.title).font(.k(15, .heavy)).foregroundColor(Theme.ink)
                HStack(spacing: 4) {
                    Image(systemName: "mappin.and.ellipse").foregroundColor(Theme.coral)
                    Text(event.place)
                }
                .font(.k(12, .medium)).foregroundColor(Theme.slate)
                GeometryReader { g in
                    ZStack(alignment: .leading) {
                        Capsule().fill(Theme.chipBg)
                        Capsule().fill(event.voted ? Theme.mint : Theme.lavenderMid)
                            .frame(width: g.size.width * min(max(fraction, 0.05), 1))
                    }
                }
                .frame(height: 4)
                .padding(.top, 2)
                .accessibilityHidden(true)
            }

            Spacer(minLength: 6)

            Button(action: onVote) {
                VStack(spacing: 2) {
                    Image(systemName: "arrow.up").font(.system(size: 14, weight: .bold))
                    Text("\(event.votes)").font(.k(13, .heavy))
                        .contentTransition(.numericText())
                }
                .foregroundColor(event.voted ? Theme.mintDark : Theme.slate)
                .frame(width: 52).padding(.vertical, 9)
                .background(RoundedRectangle(cornerRadius: 15, style: .continuous)
                    .fill(event.voted ? Theme.mint.opacity(0.16) : Color.clear))
                .overlay(RoundedRectangle(cornerRadius: 15, style: .continuous)
                    .strokeBorder(event.voted ? Theme.mint : Theme.line, lineWidth: 1.5))
                .contentShape(Rectangle())
            }
            .buttonStyle(PressStyle())
            .accessibilityLabel(event.voted ? "Remove vote for \(event.title)" : "Vote for \(event.title)")
            .accessibilityValue("\(event.votes) votes")
        }
        .card(padding: 12)
    }
}

struct EventsView: View {
    @EnvironmentObject private var store: Store

    var body: some View {
        let featured = store.featured
        let events = store.events
        let maxVotes = max(featured.votes, events.map(\.votes).max() ?? 1, 1)

        ScrollView(showsIndicators: false) {
            VStack(alignment: .leading, spacing: 16) {
                BrandHeader()
                ScreenTitle(title: "Community Events",
                            subtitle: "Vote for what happens in \(Config.homeCity) next month.")
                    .padding(.top, 4)
                FeaturedEvent(event: featured) { vote(featured) }
                SectionLabel(text: "Up next").padding(.top, 4).padding(.horizontal, 2)
                ForEach(events) { e in
                    EventRow(event: e, fraction: Double(e.votes) / Double(maxVotes)) { vote(e) }
                }
            }
            .padding(16)
        }
        .background(AmbientBackground())
    }

    private func vote(_ e: EventItem) {
        Haptics.tap()
        withAnimation(.spring(response: 0.35, dampingFraction: 0.8)) { store.toggleVote(e) }
    }
}

// MARK: - Connections + review sheet

struct ConnectionsView: View {
    @EnvironmentObject private var store: Store
    @State private var reviewing: Connection?

    var body: some View {
        let connections = store.connections
        ScrollView(showsIndicators: false) {
            VStack(alignment: .leading, spacing: 16) {
                BrandHeader()
                ScreenTitle(title: "Connections", subtitle: "Share how your meetups went.")
                    .padding(.top, 4)
                summary(pending: store.pendingReviews)
                ForEach(connections) { c in row(c) }
            }
            .padding(16)
            .animation(.spring(response: 0.4, dampingFraction: 0.85), value: store.pendingReviews)
        }
        .background(AmbientBackground())
        .sheet(item: $reviewing) { c in
            ReviewSheet(connection: c) { rating, text in
                withAnimation { store.submitReview(for: c.id, rating: rating, text: text) }
            }
            .presentationDetents([.medium, .large])
            .presentationDragIndicator(.visible)
        }
    }

    private func summary(pending: Int) -> some View {
        HStack(spacing: 12) {
            Image(systemName: pending > 0 ? "text.bubble.fill" : "party.popper.fill")
                .font(.system(size: 20))
                .foregroundColor(pending > 0 ? Theme.coral : Theme.mintDark)
                .frame(width: 44, height: 44)
                .background(RoundedRectangle(cornerRadius: 14, style: .continuous)
                    .fill(pending > 0 ? Theme.coralSoft : Theme.mint.opacity(0.16)))
                .accessibilityHidden(true)
            VStack(alignment: .leading, spacing: 2) {
                Text(pending > 0 ? "\(pending) review\(pending == 1 ? "" : "s") waiting" : "You're all caught up")
                    .font(.k(15, .heavy)).foregroundColor(Theme.ink)
                Text(pending > 0 ? "Honest reviews keep Kindred trustworthy." : "Thanks for keeping the community kind.")
                    .font(.k(12.5)).foregroundColor(Theme.slate)
            }
            Spacer()
        }
        .card(padding: 14)
        .accessibilityElement(children: .combine)
    }

    private func row(_ c: Connection) -> some View {
        HStack(alignment: .center, spacing: 12) {
            Avatar(initials: c.initials, tone: c.tone, portrait: c.portrait)
            VStack(alignment: .leading, spacing: 4) {
                Text(c.name).font(.k(15, .heavy)).foregroundColor(Theme.ink)
                Text(c.detail).font(.k(12, .medium)).foregroundColor(Theme.slate)
                if let r = c.rating { StarRow(rating: r) }
                if let text = c.review {
                    Text("“\(text)”")
                        .font(.k(12.5)).italic().foregroundColor(Theme.slateDark)
                        .lineLimit(2).padding(.top, 1)
                }
            }
            Spacer(minLength: 6)
            Button { reviewing = c } label: {
                if c.rating == nil {
                    ActionButton(title: "Review")
                } else {
                    ActionButton(title: "Edit", icon: "pencil", kind: .lavender)
                }
            }
            .buttonStyle(PressStyle())
            .accessibilityLabel(c.rating == nil ? "Review \(c.name)" : "Edit review for \(c.name)")
        }
        .card(padding: 14)
    }
}

struct ReviewSheet: View {
    let connection: Connection
    let onSubmit: (Int, String) -> Void

    @Environment(\.dismiss) private var dismiss
    @State private var rating: Int
    @State private var text: String
    @FocusState private var focused: Bool

    private let labels = ["Tap a star to rate", "Not great", "It was okay", "Pretty good", "Really great", "Loved it!"]
    private var isEditing: Bool { connection.rating != nil }

    init(connection: Connection, onSubmit: @escaping (Int, String) -> Void) {
        self.connection = connection
        self.onSubmit = onSubmit
        _rating = State(initialValue: connection.rating ?? 0)
        _text = State(initialValue: connection.review ?? "")
    }

    var body: some View {
        ScrollView {
            VStack(spacing: 14) {
                Avatar(initials: connection.initials, tone: connection.tone, size: 64, portrait: connection.portrait)
                Text("How was it with \(connection.firstName)?")
                    .font(.k(22, .heavy)).foregroundColor(Theme.ink)
                    .multilineTextAlignment(.center)
                Text("Your honest review helps build community trust.")
                    .font(.k(13)).foregroundColor(Theme.slate)
                    .multilineTextAlignment(.center)

                VStack(spacing: 6) {
                    StarRow(rating: rating, size: 36, spacing: 8) { value in
                        Haptics.tap()
                        withAnimation(.spring(response: 0.3, dampingFraction: 0.6)) { rating = value }
                    }
                    Text(labels[rating])
                        .font(.k(13.5, .bold))
                        .foregroundColor(rating == 0 ? Theme.muted : Theme.coral)
                }
                .padding(.vertical, 6)

                VStack(alignment: .trailing, spacing: 6) {
                    ZStack(alignment: .topLeading) {
                        TextEditor(text: $text)
                            .focused($focused)
                            .scrollContentBackground(.hidden)
                            .font(.k(14)).foregroundColor(Color(hex: 0x334155))
                            .multilineTextAlignment(.leading)
                            .frame(minHeight: 100)
                            .padding(8)
                            .accessibilityLabel("Review")
                        if text.isEmpty {
                            Text("Write a few words about how it went…")
                                .font(.k(14)).foregroundColor(Theme.muted)
                                .padding(.horizontal, 13).padding(.vertical, 16)
                                .allowsHitTesting(false)
                                .accessibilityHidden(true)
                        }
                    }
                    .background(RoundedRectangle(cornerRadius: 18, style: .continuous).fill(Color(hex: 0xFAF9FF)))
                    .overlay(RoundedRectangle(cornerRadius: 18, style: .continuous)
                        .strokeBorder(focused ? Theme.inkLight : Theme.lavenderMid.opacity(0.7), lineWidth: 1.5))
                    .animation(.easeOut(duration: 0.15), value: focused)

                    Text("\(text.count)/\(Config.reviewLimit)")
                        .font(.k(11.5, .semibold)).monospacedDigit()
                        .foregroundColor(text.count >= Config.reviewLimit ? Theme.coral : Theme.muted)
                        .padding(.trailing, 6)
                }
                .onChange(of: text) { newValue in
                    if newValue.count > Config.reviewLimit {
                        text = String(newValue.prefix(Config.reviewLimit))
                    }
                }

                Button {
                    Haptics.success()
                    onSubmit(rating, text.trimmingCharacters(in: .whitespacesAndNewlines))
                    dismiss()
                } label: { ActionButton(title: isEditing ? "Update review" : "Submit review", kind: .coral, full: true) }
                    .buttonStyle(PressStyle())
                    .disabled(rating == 0)
                    .opacity(rating == 0 ? 0.45 : 1)
                    .accessibilityHint(rating == 0 ? "Choose a star rating first" : "")
            }
            .padding(.horizontal, 22).padding(.top, 28).padding(.bottom, 18)
        }
        .background(Theme.canvas.ignoresSafeArea())
        .scrollDismissesKeyboard(.interactively)
        .toolbar {
            ToolbarItemGroup(placement: .keyboard) {
                Spacer()
                Button("Done") { focused = false }
            }
        }
    }
}

// MARK: - Profile + Face ID verification

struct ProfileView: View {
    @EnvironmentObject private var store: Store
    @State private var showFaceID = false
    @State private var pickerItem: PhotosPickerItem?

    var body: some View {
        ScrollView(showsIndicators: false) {
            VStack(spacing: 16) {
                BrandHeader()
                headerCard
                interestsCard
                reviewsCard
                verifiedCard
            }
            .padding(16)
        }
        .background(AmbientBackground())
        .fullScreenCover(isPresented: $showFaceID) { FaceIDView() }
        .onChange(of: pickerItem) { item in
            guard let item else { return }
            Task { @MainActor in
                if let data = try? await item.loadTransferable(type: Data.self) { store.setPhoto(data) }
                pickerItem = nil
            }
        }
    }

    private var headerCard: some View {
        VStack(spacing: 0) {
            ZStack(alignment: .bottom) {
                ZStack {
                    Theme.brand
                    Circle().fill(Color.white.opacity(0.10)).frame(width: 160, height: 160).offset(x: 120, y: -30)
                    Circle().fill(Theme.coral.opacity(0.35)).frame(width: 100, height: 100)
                        .blur(radius: 24).offset(x: -110, y: 30)
                }
                .frame(height: 96).clipped()

                PhotosPicker(selection: $pickerItem, matching: .images) {
                    Avatar(initials: "YO", tone: 0, size: 84, verified: true, image: store.myPhoto)
                        .overlay(alignment: .bottomLeading) {
                            Image(systemName: "camera.fill")
                                .font(.system(size: 11, weight: .bold)).foregroundColor(.white)
                                .frame(width: 26, height: 26)
                                .background(Circle().fill(Theme.coralGradient))
                                .overlay(Circle().strokeBorder(Color.white, lineWidth: 2))
                                .offset(x: -6, y: 6)
                        }
                        .padding(4)
                        .background(RoundedRectangle(cornerRadius: 84 * 0.36 + 4, style: .continuous).fill(Color.white))
                }
                .buttonStyle(PressStyle())
                .contextMenu {
                    if store.myPhoto != nil {
                        Button(role: .destructive) { store.removePhoto() } label: {
                            Label("Remove photo", systemImage: "trash")
                        }
                    }
                }
                .accessibilityLabel("Change profile photo")
                .offset(y: 44)
            }

            VStack(spacing: 6) {
                Text("You").font(.k(24, .heavy)).foregroundColor(Theme.ink)
                PersonalityPill(code: "ENFP", type: "Explorer")
                if store.myPhoto == nil {
                    Text("Tap your picture to add a photo")
                        .font(.k(11.5, .medium)).foregroundColor(Theme.muted)
                }
            }
            .padding(.top, 52)

            HairLine().padding(.top, 16)

            HStack(spacing: 0) {
                stat("\(store.connections.count)", "Connections")
                stat("\(store.votedCount)", "Votes cast")
                stat(RatingSummary(store.reviewsAboutMe).label, "Avg. rating")
            }
            .padding(.vertical, 14)
        }
        .cardChrome(radius: 26)
    }

    private func stat(_ value: String, _ label: String) -> some View {
        VStack(spacing: 2) {
            Text(value).font(.k(20, .heavy)).foregroundColor(Theme.ink)
                .contentTransition(.numericText())
            Text(label).font(.k(11.5, .medium)).foregroundColor(Theme.slate)
        }
        .frame(maxWidth: .infinity)
        .accessibilityElement(children: .combine)
    }

    private var interestsCard: some View {
        VStack(alignment: .leading, spacing: 10) {
            HStack {
                SectionLabel(text: "My interests")
                Spacer()
                Text("\(store.myInterests.count) selected")
                    .font(.k(11.5, .bold)).foregroundColor(Theme.inkLight)
            }
            Text("Tap to add or remove. Your matches update instantly.")
                .font(.k(12.5)).foregroundColor(Theme.slate)
            FlowLayout(spacing: 8, lineSpacing: 8) {
                ForEach(Person.allInterests, id: \.self) { tag in
                    Button {
                        Haptics.tap()
                        withAnimation(.easeInOut(duration: 0.2)) { store.toggleInterest(tag) }
                    } label: { Chip(text: tag, icon: Person.icon(for: tag), on: store.myInterests.contains(tag)) }
                    .buttonStyle(PressStyle())
                }
            }
            .padding(.top, 2)
        }
        .frame(maxWidth: .infinity, alignment: .leading).card(padding: 18)
    }

    private var reviewsCard: some View {
        let list = store.reviewsAboutMe
        let summary = RatingSummary(list)
        return VStack(alignment: .leading, spacing: 12) {
            HStack {
                SectionLabel(text: "What people say about you")
                Spacer()
                RatingBadge(summary: summary)
            }
            RatingSummaryView(summary: summary).padding(.vertical, 4)
            ForEach(list) { r in ReviewCard(review: r, plain: true) }
        }
        .frame(maxWidth: .infinity, alignment: .leading).card(padding: 18)
    }

    private var verifiedCard: some View {
        HStack(spacing: 12) {
            Image(systemName: "faceid").font(.system(size: 24)).foregroundColor(Theme.mintDark)
                .frame(width: 48, height: 48)
                .background(RoundedRectangle(cornerRadius: 15, style: .continuous).fill(Theme.mint.opacity(0.16)))
                .accessibilityHidden(true)
            VStack(alignment: .leading, spacing: 2) {
                Text("Face ID verified").font(.k(15, .heavy)).foregroundColor(Theme.ink)
                Text("Your identity is confirmed.").font(.k(12.5)).foregroundColor(Theme.slate)
            }
            Spacer()
            Button { showFaceID = true } label: { ActionButton(title: "View", kind: .lavender) }
                .buttonStyle(PressStyle())
                .accessibilityLabel("View verification details")
        }
        .card(padding: 16)
    }
}

struct Bracket: Shape {
    func path(in rect: CGRect) -> Path {
        var p = Path()
        p.move(to: CGPoint(x: 0, y: rect.height))
        p.addLine(to: CGPoint(x: 0, y: 16))
        p.addQuadCurve(to: CGPoint(x: 16, y: 0), control: .zero)
        p.addLine(to: CGPoint(x: rect.width, y: 0))
        return p
    }
}

struct ScanFrame: View {
    let done: Bool
    @State private var down = false
    @State private var pulse = false
    @Environment(\.accessibilityReduceMotion) private var reduceMotion
    private let corners: [Alignment] = [.topLeading, .topTrailing, .bottomTrailing, .bottomLeading]

    var body: some View {
        ZStack {
            ForEach(0..<4, id: \.self) { i in
                Bracket()
                    .stroke(Theme.mint, style: StrokeStyle(lineWidth: 3, lineCap: .round))
                    .frame(width: 34, height: 34)
                    .rotationEffect(.degrees(Double(i) * 90))
                    .frame(maxWidth: .infinity, maxHeight: .infinity, alignment: corners[i])
            }
            // Soft pulse ring while scanning
            Circle()
                .stroke(Theme.mint.opacity(0.45), lineWidth: 2)
                .padding(22)
                .scaleEffect(pulse ? 1.16 : 1)
                .opacity(done ? 0 : (pulse ? 0 : 0.9))
            Circle()
                .fill(RadialGradient(colors: [Color(hex: 0x5B52E6), Color(hex: 0x1E1B4B)],
                                     center: .init(x: 0.5, y: 0.3), startRadius: 4, endRadius: 150))
                .shadow(color: Color(hex: 0x818CF8).opacity(0.5), radius: 30)
                .padding(22)
            ZStack {
                Image(systemName: done ? "checkmark" : "faceid")
                    .font(.system(size: done ? 80 : 92, weight: done ? .bold : .ultraLight))
                    .foregroundColor(done ? Theme.mint : Theme.lavenderMid.opacity(0.6))
                    .scaleEffect(done ? 1 : 0.96)
                if !done {
                    GeometryReader { geo in
                        Capsule()
                            .fill(LinearGradient(colors: [.clear, Theme.mint, .clear], startPoint: .leading, endPoint: .trailing))
                            .frame(height: 3)
                            .shadow(color: Theme.mint, radius: 8)
                            .offset(y: down ? geo.size.height * 0.88 : geo.size.height * 0.1)
                    }
                }
            }
            .clipShape(Circle())
            .padding(22)
            Circle().stroke(done ? Theme.mint : Theme.lavenderMid.opacity(0.45), lineWidth: 3).padding(22)
        }
        .frame(width: 250, height: 250)
        .animation(.spring(response: 0.45, dampingFraction: 0.7), value: done)
        .onAppear {
            guard !reduceMotion else { return }
            withAnimation(.easeInOut(duration: 2.6).repeatForever(autoreverses: true)) { down = true }
            withAnimation(.easeOut(duration: 1.7).repeatForever(autoreverses: false)) { pulse = true }
        }
        .accessibilityHidden(true)
    }
}

struct FaceIDView: View {
    @Environment(\.dismiss) private var dismiss
    @State private var progress = 0.0
    @State private var scanID = 0

    private var done: Bool { progress >= 1 }

    var body: some View {
        ZStack {
            LinearGradient(colors: [Color(hex: 0x1E1B4B), Theme.ink, Theme.inkLight],
                           startPoint: .top, endPoint: .bottom)
                .ignoresSafeArea()
            Circle().fill(Theme.coral.opacity(0.18)).frame(width: 280, height: 280)
                .blur(radius: 80).offset(x: 130, y: 260)
            VStack(spacing: 0) {
                HStack(spacing: 6) {
                    ForEach(0..<4, id: \.self) { segment($0) }
                }
                .padding(.horizontal, 24).padding(.top, 12)
                .accessibilityHidden(true)

                Text(done ? "You're verified" : "Verify it's really you")
                    .font(.k(26, .heavy)).foregroundColor(.white).padding(.top, 28)
                    .animation(nil, value: done)
                    .accessibilityAddTraits(.isHeader)
                Text("A quick face scan keeps Kindred safe. Your scan is never shown to other members.")
                    .font(.k(13.5)).foregroundColor(Theme.lavender.opacity(0.78))
                    .multilineTextAlignment(.center).lineSpacing(3)
                    .padding(.horizontal, 34).padding(.top, 8)

                ScanFrame(done: done).padding(.top, 30)

                HStack(spacing: 7) {
                    Image(systemName: done ? "checkmark.seal.fill" : "checkmark.shield.fill")
                    Text(done ? "Verified" : "Scanning… \(Int(progress * 100))%")
                        .monospacedDigit()
                }
                .font(.k(12.5, .bold)).foregroundColor(Color(hex: 0x6EE7B7))
                .padding(.horizontal, 15).padding(.vertical, 8)
                .background(Capsule().fill(Theme.mint.opacity(0.16)))
                .padding(.top, 22)
                .accessibilityElement(children: .combine)

                Spacer()

                Button { dismiss() } label: {
                    ActionButton(title: done ? "Continue" : "Cancel", kind: done ? .coral : .lavender, full: true)
                }
                .buttonStyle(PressStyle())
                .padding(.horizontal, 24)

                Button {
                    Haptics.tap()
                    scanID += 1
                } label: {
                    Text("Scan again").font(.k(13.5, .semibold))
                        .foregroundColor(Theme.lavender.opacity(0.85))
                        .padding(.vertical, 10)
                }
                .opacity(done ? 1 : 0)
                .disabled(!done)

                HStack(spacing: 6) {
                    Image(systemName: "lock.fill")
                    Text("Encrypted & deleted after verification")
                }
                .font(.k(11.5, .medium)).foregroundColor(Theme.lavender.opacity(0.7))
                .padding(.bottom, 16)
            }
        }
        .preferredColorScheme(.dark)
        .task(id: scanID) {
            progress = 0
            while progress < 1 {
                try? await Task.sleep(nanoseconds: 40_000_000)
                if Task.isCancelled { return }
                withAnimation(.linear(duration: 0.04)) { progress = min(progress + 0.012, 1) }
            }
            Haptics.success()   // this view is MainActor-isolated, so no hop needed
        }
    }

    private func segment(_ i: Int) -> some View {
        let fill = min(max(progress * 4 - Double(i), 0), 1)
        return Capsule().fill(Theme.mint.opacity(0.25)).frame(height: 5)
            .overlay(alignment: .leading) {
                GeometryReader { g in
                    Capsule().fill(Theme.mint).frame(width: g.size.width * fill)
                }
            }
    }
}
