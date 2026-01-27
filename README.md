@@ -0,0 +1,174 @@
//
//  ContentView.swift
//  GUIDE TO WLV
//
//  Created by Mercy Ekuban on 27/01/2026.
//

import SwiftUI

struct ContentView: View {
    var body: some View {
        NavigationStack {
            VStack {
                Image("logo")
                    .resizable()
                    .scaledToFit()
                    .frame(width: 50)

                Text("Hello, Welcome to GUIDE TO WLV!")
            }
            .padding()

            NavigationLink("Login") {
                LoginView()
            }

            NavigationLink("Register") {
                RegisterView()
            }
            .padding()
        }
    }
}

struct LoginView: View {
    @State private var email = ""
    @State private var password = ""

    var body: some View {
        VStack(spacing: 16) {
            Text("Login").font(.title).bold()

            TextField("Email", text: $email)
                .textContentType(.username)
                .keyboardType(.emailAddress)
                .textInputAutocapitalization(.never)
                .autocorrectionDisabled()
                .padding(12)
                .background(.thinMaterial, in: RoundedRectangle(cornerRadius: 12))

            SecureField("Password", text: $password)
                .textContentType(.password)
                .padding(12)
                .background(.thinMaterial, in: RoundedRectangle(cornerRadius: 12))

            Button("Sign In") {
                // Call your auth here
            }
            .frame(maxWidth: .infinity)
            .padding(.vertical, 12)
            .background(Color.accentColor, in: RoundedRectangle(cornerRadius: 12))
            .foregroundStyle(.white)

            Spacer()
        }
        .padding()
        .navigationTitle("Login")
        .navigationBarTitleDisplayMode(.inline)
    }
}

struct RegisterView: View {
    @State private var fullName = ""
    @State private var email = ""
    @State private var password = ""
    @State private var confirmPassword = ""
    @State private var isRegistering = false
    @State private var errorMessage: String?

    var body: some View {
        ScrollView {
            VStack(spacing: 16) {
                Text("Create Account")
                    .font(.title)
                    .bold()

                TextField("Full name", text: $fullName)
                    .textContentType(.name)
                    .textInputAutocapitalization(.words)
                    .padding(12)
                    .background(.thinMaterial, in: RoundedRectangle(cornerRadius: 12))

                TextField("Email", text: $email)
                    .textContentType(.username)
                    .keyboardType(.emailAddress)
                    .textInputAutocapitalization(.never)
                    .autocorrectionDisabled()
                    .padding(12)
                    .background(.thinMaterial, in: RoundedRectangle(cornerRadius: 12))

                SecureField("Password", text: $password)
                    .textContentType(.newPassword)
                    .padding(12)
                    .background(.thinMaterial, in: RoundedRectangle(cornerRadius: 12))

                SecureField("Confirm Password", text: $confirmPassword)
                    .textContentType(.newPassword)
                    .padding(12)
                    .background(.thinMaterial, in: RoundedRectangle(cornerRadius: 12))

                if let errorMessage {
                    Text(errorMessage)
                        .foregroundStyle(.red)
                        .font(.footnote)
                        .multilineTextAlignment(.center)
                }

                Button {
                    Task { await register() }
                } label: {
                    HStack {
                        if isRegistering { ProgressView().tint(.white) }
                        Text("Register").bold()
                    }
                    .frame(maxWidth: .infinity)
                    .padding(.vertical, 12)
                    .background(Color.accentColor, in: RoundedRectangle(cornerRadius: 12))
                    .foregroundStyle(.white)
                }
                .disabled(isRegistering || !canSubmit)

                Spacer(minLength: 0)
            }
            .padding()
        }
        .navigationTitle("Register")
        .navigationBarTitleDisplayMode(.inline)
    }

    private var canSubmit: Bool {
        !fullName.isEmpty && !email.isEmpty && !password.isEmpty && password == confirmPassword
    }

    // Replace this with your real registration logic
    private func register() async {
        errorMessage = nil
        guard canSubmit else {
            errorMessage = "Please fill all fields and make sure passwords match."
            return
        }

        isRegistering = true
        defer { isRegistering = false }

        // Simulate network delay
        try? await Task.sleep(nanoseconds: 1_000_000_000)

        // Example validation
        guard email.contains("@") else {
            errorMessage = "Please enter a valid email address."
            return
        }

        // Success path (hook up to your backend here)
        fullName = ""
        email = ""
        password = ""
        confirmPassword = ""
    }
}

#Preview {
    ContentView()
}
