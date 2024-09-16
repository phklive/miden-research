![Miden research logo](./src/assets/images/miden_research.jpeg)

This mdbook aims to provide insights into the future of the Miden protocol.

## Live website

You can find a live website of the Miden research book [here](https://phklive.github.io/miden-research/)

## Running the mdbook

To run the mdbook locally, follow these steps:

1. Make sure you have Rust installed on your system. If not, you can install it from [https://www.rust-lang.org/tools/install](https://www.rust-lang.org/tools/install).

2. Install mdbook by running the following command:

   ```
   cargo install mdbook
   ```

3. Clone this repository:

   ```
   git clone https://github.com/your-username/miden-research.git
   cd miden-research
   ```

4. Build and serve the book:
   ```
   mdbook serve --open
   ```

This command will build the book and open it in your default web browser. The `--open` flag automatically opens the book in your browser.

## Reading the mdbook

Once you have the mdbook running, you can read it in the following ways:

1. **Local Browser**: If you're running the book locally using `mdbook serve`, it will be available at `http://localhost:3000` by default.

2. **Navigation**: Use the sidebar on the left to navigate between different chapters and sections of the book.

3. **Search**: Utilize the search bar at the top to find specific topics or keywords within the book.

4. **Offline Reading**: You can also generate a static version of the book for offline reading:
   ```
   mdbook build
   ```
   This will create a `book` directory with HTML files that you can open in any web browser.

## Contributing

We welcome contributions to this research. Please feel free to submit pull requests or open issues for discussion.

## License

This project is licensed under the [MIT License](LICENSE).
