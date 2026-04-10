# Data-Structures-Playground-Swift-UI-
Developed a macOS SwiftUI app to visualize key data structures (arrays, stacks, queues, linked lists) with interactive operations such as insertion, deletion, traversal, and stack push/pop.

# Data Structures Playground

A SwiftUI macOS app that visualizes arrays, stacks, queues, and linked lists.

## Features
- Array operations (add, remove, search)
- Stack (push/pop/peek)
- Queue (enqueue/dequeue/peek)
- Real-time output log

## Tech Stack
- Swift
- SwiftUI


Clone the repo
Open in Xcode
Run on macOS or iOS simulator
Copy and Paste the below code into the Xcode IDE


import SwiftUI

struct ContentView: View {
    
    // Data Structures
    @State private var numbers: [Int] = []
    @State private var stack: [Int] = []
    @State private var queue: [Int] = []
    
    // UI State
    @State private var inputNumber: String = ""
    @State private var outputText: [String] = ["Welcome to Data Structures Playground!"]
    
    var body: some View {
        ScrollView(.vertical, showsIndicators: true) { // <-- Top-level scroll
            VStack(spacing: 15) {
                
                Text("📊 Data Structures Playground")
                    .font(.title2)
                    .padding(.top)
                
                // INPUT
                HStack {
                    TextField("Enter number", text: $inputNumber)
                        .textFieldStyle(RoundedBorderTextFieldStyle())
                        .frame(width: 120)
                }
                
                Divider()
                
                // 🟡 ARRAY
                VStack {
                    Text("🟡 Array").bold()
                    
                    HStack(spacing: 8) {
                        ForEach(numbers.indices, id: \.self) { index in
                            VStack {
                                Text("\(numbers[index])")
                                    .frame(width: 50, height: 40)
                                    .background(Color.orange)
                                    .foregroundColor(.white)
                                    .cornerRadius(6)
                                
                                Text("\(index)")
                                    .font(.caption)
                                    .foregroundColor(.gray)
                            }
                        }
                    }
                    .padding()
                    .border(Color.black)
                    
                    HStack {
                        Button("Add") { addNumber() }
                        Button("Remove") { removeNumber() }
                        Button("Count") { countOccurrences() }
                        Button("Show") { showNumbers() }
                        Button("Max") { findMax() }
                        Button("Min") { findMin() }
                    }
                }
                
                Divider()
                
                // 🟢 STACK
                VStack {
                    Text("🟢 Stack (LIFO)").bold()
                    
                    VStack(spacing: 8) {
                        ForEach(stack.indices.reversed(), id: \.self) { index in
                            Text("\(stack[index])")
                                .frame(width: 100, height: 40)
                                .background(index == stack.count - 1 ? Color.green : Color.blue)
                                .foregroundColor(.white)
                                .cornerRadius(8)
                        }
                    }
                    .padding()
                    .border(Color.black)
                    
                    HStack {
                        Button("Push") { pushStack() }
                        Button("Pop") { popStack() }
                        Button("Peek") { peekStack() }
                        Button("Show") { showStack() }
                    }
                }
                
                Divider()
                
                // 🔵 QUEUE
                VStack {
                    Text("🔵 Queue (FIFO)").bold()
                    
                    HStack(spacing: 8) {
                        ForEach(queue.indices, id: \.self) { index in
                            Text("\(queue[index])")
                                .frame(width: 50, height: 40)
                                .background(index == 0 ? Color.green : Color.blue)
                                .foregroundColor(.white)
                                .cornerRadius(6)
                        }
                    }
                    .padding()
                    .border(Color.black)
                    
                    HStack {
                        Button("Enqueue") { enqueueQueue() }
                        Button("Dequeue") { dequeueQueue() }
                        Button("Peek") { peekQueue() }
                        Button("Show") { showQueue() }
                    }
                }
                
                Divider()
                
                // 📜 OUTPUT LOG
                VStack(alignment: .leading) {
                    Text("📜 Output Log").bold()
                    
                    VStack(alignment: .leading, spacing: 2) {
                        ForEach(outputText.indices, id: \.self) { index in
                            Text(outputText[index])
                                .frame(maxWidth: .infinity, alignment: .leading)
                        }
                    }
                    .padding()
                    .border(Color.gray)
                    
                    HStack {
                        Button("Clear Output") {
                            outputText.removeAll()
                        }
                    }
                }
                
            }
            .padding()
        }
        .frame(minWidth: 700, minHeight: 750)
    }
    
    // MARK: - ARRAY FUNCTIONS
    func addNumber() {
        if let num = Int(inputNumber) {
            withAnimation { numbers.append(num) }
            appendOutput("✅ Added \(num) to Array")
            inputNumber = ""
        } else { appendOutput("❌ Invalid input") }
    }
    
    func removeNumber() {
        if let num = Int(inputNumber) {
            if let index = numbers.firstIndex(of: num) {
                withAnimation { numbers.remove(at: index) }
                appendOutput("🗑 Removed \(num)")
            } else { appendOutput("⚠️ Not found") }
            inputNumber = ""
        }
    }
    
    func showNumbers() { appendOutput("Array: \(numbers)") }
    func findMax() { appendOutput(numbers.max().map { "Max: \($0)" } ?? "Array empty") }
    func findMin() { appendOutput(numbers.min().map { "Min: \($0)" } ?? "Array empty") }
    func countOccurrences() {
        if let num = Int(inputNumber) {
            let count = numbers.filter { $0 == num }.count
            appendOutput("\(num) occurs \(count) times")
            inputNumber = ""
        }
    }
    
    // MARK: - STACK FUNCTIONS
    func pushStack() {
        if let num = Int(inputNumber) {
            withAnimation { stack.append(num) }
            appendOutput("📥 Pushed \(num)")
            inputNumber = ""
        }
    }
    
    func popStack() {
        if !stack.isEmpty {
            withAnimation { let removed = stack.removeLast(); appendOutput("📤 Popped \(removed)") }
        } else { appendOutput("⚠️ Stack empty") }
    }
    
    func peekStack() { appendOutput(stack.last.map { "🔎 Top: \($0)" } ?? "⚠️ Stack empty") }
    func showStack() { appendOutput("Stack: \(stack)") }
    
    // MARK: - QUEUE FUNCTIONS
    func enqueueQueue() {
        if let num = Int(inputNumber) {
            withAnimation { queue.append(num) }
            appendOutput("📥 Enqueued \(num)")
            inputNumber = ""
        }
    }
    
    func dequeueQueue() {
        if !queue.isEmpty {
            withAnimation { let removed = queue.removeFirst(); appendOutput("📤 Dequeued \(removed)") }
        } else { appendOutput("⚠️ Queue empty") }
    }
    
    func peekQueue() { appendOutput(queue.first.map { "🔎 Front: \($0)" } ?? "⚠️ Queue empty") }
    func showQueue() { appendOutput("Queue: \(queue)") }
    
    // MARK: - HELPER
    func appendOutput(_ text: String) { outputText.append(text) }
}

// Preview
#Preview { ContentView() }
