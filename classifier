var video;				// webcam
var featureExtractor;	// feature extractor
var classifier;			// image classifier

// buttons
var backButton;	// background button
var mugButton;
var hatButton;
var trainButton;

// counts
var backCount = 0;
var mugCount = 0;
var hatCount = 0;

function backButtonPressed() {
	status("backButtonPressed");
	classifier.addImage("background");
	backCount += 1;
	status("background trained: " + backCount.toString());
};

function mugButtonPressed() {
	status("mugButtonPressed");
	classifier.addImage("mug");
	mugCount += 1;
	status("mugs trained: " + mugCount.toString());
};

function hatButtonPressed() {
	status("hatButtonPressed");
	classifier.addImage("hat");
	hatCount += 1;
	status("hats trained: " + hatCount.toString());
};

function trainButtonPressed() {
	tp = classifier.train(whileTraining);
	tp.catch((error) => { status(error); });
}

function whileTraining(loss) {
	if (loss != null) {
		status("Training loss: " + loss.toString());
	} else {
		status("Training done.");
		classifier.classify(gotResults);
	}
}
function gotResults(error, results) {
	if (error) {
		status("ERROR: " + error);
	} else {
		status("Guess: " + results[0].label);
		classifier.classify(gotResults);
	}
}

function modelReady() {
	status("Model loaded and ready.");
}

function videoReady() {
	status("Video ready.");
}

function setup() {
	status("Setting up.");

	// create a P5 canvas to draw on
	createCanvas(640, 480, document.getElementById("webcam"));
	background(0);	// 0 = black

	// setup webcam and background
	// VIDEO is a constant that will default to the default webcam
	video = createCapture(VIDEO);
	video.hide();        // hide the video so we can control its placement

	// create feature extractor and classifier
	featureExtractor = ml5.featureExtractor('MobileNet', {
		numLabels: 3,
	}, modelReady);
	classifier = featureExtractor.classification(video, videoReady);

	// add training buttons
	backButton = createButton("background");
	backButton.size(100, 25);
	backButton.mousePressed(backButtonPressed);

	mugButton = createButton("mug");
	mugButton.size(100, 25);
	mugButton.mousePressed(mugButtonPressed);

	hatButton = createButton("hat");
	hatButton.size(100, 25);
	hatButton.mousePressed(hatButtonPressed);

	trainButton = createButton("train");
	trainButton.size(100, 25);
	trainButton.mousePressed(trainButtonPressed);
}

function draw() {

	// mirror the image on the x-axis
	translate(width, 0); // move to far corner (right side)
	scale(-1.0, 1.0);    // flip x-axis backwards

	// draw video as image to control its location on the canvas
	image(video, 0, 0);
}

function status(s) {
	console.log(s);
	var status = document.getElementById("status")
	status.innerHTML = s;
}
