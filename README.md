# APUpdate

Uses this code to update your Cocoa application

and your storyboard should look like the screenshot image in the files

```Swift
import Cocoa

class UNUpdaterViewController: NSViewController {

    @IBOutlet var featureListTextView: NSTextView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do view setup here.
        do {
            /// Create a .txt file in github, and then get the download url and replace it with this
            let contents = try? String(contentsOf: URL(string: "https://raw.githubusercontent.com/Ashwin-Paudel/download/main/newupdateinfo.txt")!)
            // If we can't recive the text, we will write and error
            featureListTextView.string = contents ?? "Error Loading Features"
        } catch {
        }
//        featureListTextView.string = "hihihi"
    }
    @IBAction func updateButtonPress(_ sender: Any) {
        // MARK: - Stage 1: Delete the app
        do {
            // Replace Unique.app with your_app_name.app
            try FileManager.default.removeItem(atPath: "/Applications/Unique.app")
        } catch {
            print("Error")
        }
        // MARK: - Stage 2: Download the new app
        /// Compress your app in a zip, upload to github and get download link
        /// Replace it with the zip download link
        let url = URL(string: "https://github.com/Ashwin-Paudel/download/raw/main/Unique.zip")
        FileDownloader.loadFileAsync(url: url!) { [self] (path, error) in
            
// MARK: - Unzip the app and save to application folder
            print("PDF File downloaded to : \(path!)")
            unzipFile(at: path!, to: "/Applications")
        }
    }
    func unzipFile(at sourcePath: String, to destinationPath: String) -> Bool {
        let process = Process.launchedProcess(launchPath: "/usr/bin/unzip", arguments: ["-o", sourcePath, "-d", destinationPath])
        process.waitUntilExit()
        return process.terminationStatus <= 1
    }
}

```

