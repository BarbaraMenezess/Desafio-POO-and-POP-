import UIKit
// Object-oriented programming

// Enum to represent task states
enum TaskState {
    case pending
    case inProgress
    case completed
}

// Struct representing a task
struct Task {
    var title: String
    var state: TaskState
    var assignedTo: String?

    // Method to start the task
    mutating func startTask() {
        if state == .pending {
            state = .inProgress
            print("Task \(title) has started.")
        } else {
            print("Task \(title) is already in progress or completed.")
        }
    }

    // Method to complete the task
    mutating func completeTask() {
        if state == .inProgress {
            state = .completed
            print("Task \(title) has been completed.")
        } else {
            print("Task \(title) cannot be completed because it is not in progress.")
        }
    }
}

// Closure that takes a task and prints its state
let printTaskStatus: (Task) -> Void = { task in
    print("Task \(task.title) is in the state \(task.state).")
}

// Function that executes a list of tasks concurrently
func executeTasksConcurrently(tasks: [Task], completion: @escaping () -> Void) {
    let queue = DispatchQueue(label: "com.example.taskQueue", attributes: .concurrent)
    let group = DispatchGroup()

    for var task in tasks {
        queue.async(group: group) {
            task.startTask()
            task.completeTask()
            printTaskStatus(task)
        }
    }

    group.notify(queue: .main) {
        completion()
    }
}

// Create some tasks
let task1 = Task(title: "Read a book", state: .pending, assignedTo: "Alice")
let task2 = Task(title: "Write a report", state: .pending, assignedTo: "Bob")
let task3 = Task(title: "Do exercises", state: .pending)

// Add tasks to a collection
let tasks = [task1, task2, task3]

// Print the initial state of tasks
for task in tasks {
    printTaskStatus(task)
}

// Execute tasks concurrently
executeTasksConcurrently(tasks: tasks) {
    print("All tasks have been completed.")
}
//


// Protocol-oriented programming

import Foundation

// Enum to represent music genres
enum MusicGenre: String, CaseIterable {
    case pop
    case rock
    case hipHop
    case classical
    case jazz
}

// Protocol to represent a music track
protocol MusicTrack {
    var title: String { get }
    var artist: String { get }
    var genre: MusicGenre { get }
    func play()
}

// Struct representing a basic music track conforming to the MusicTrack protocol
struct BasicMusicTrack: MusicTrack {
    var title: String
    var artist: String
    var genre: MusicGenre
    
    func play() {
        print("Now playing: \(title) by \(artist) in \(genre.rawValue) genre.")
    }
}

// Struct representing a user's music preferences
struct UserPreferences {
    var favoriteGenres: Set<MusicGenre> = []
    
    mutating func addFavoriteGenre(_ genre: MusicGenre) {
        favoriteGenres.insert(genre)
    }
}

// Function to simulate concurrent music playback based on user preferences
func simulateConcurrentPlayback(tracks: [MusicTrack], userPreferences: UserPreferences, completion: @escaping () -> Void) {
    let queue = DispatchQueue(label: "com.example.musicQueue", attributes: .concurrent)
    let group = DispatchGroup()

    for track in tracks {
        queue.async(group: group) {
            if userPreferences.favoriteGenres.contains(track.genre) {
                track.play()
            }
        }
    }

    group.notify(queue: .main) {
        completion()
    }
}

// Example usage
let track1 = BasicMusicTrack(title: "Shape of You", artist: "Ed Sheeran", genre: .pop)
let track2 = BasicMusicTrack(title: "Bohemian Rhapsody", artist: "Queen", genre: .rock)
let track3 = BasicMusicTrack(title: "Sicko Mode", artist: "Travis Scott", genre: .hipHop)
let track4 = BasicMusicTrack(title: "Moonlight Sonata", artist: "Beethoven", genre: .classical)
let track5 = BasicMusicTrack(title: "Take Five", artist: "Dave Brubeck", genre: .jazz)

let tracks = [track1, track2, track3, track4, track5]

var userPreferences = UserPreferences()
userPreferences.addFavoriteGenre(.pop)
userPreferences.addFavoriteGenre(.jazz)

simulateConcurrentPlayback(tracks: tracks, userPreferences: userPreferences) {
    print("Playback completed based on user preferences.")
}

