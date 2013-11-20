# ofxBeat

A simple beat detection routine for openFrameworks that gives 32 fft bands
plus separate output for kick, snare, and hihat. Created for inclusion in the
Cyril language for live coded visuals.

## Provenance

This builds on the beat tracking code in [https://github.com/zenwerk/ofxBeatTracking](ofxBeatTracking), which itself was based on C++ code from Dominic Mazzoni, which was based on an free FFT implementation in Fortan. Further details
are in the comments [https://github.com/zenwerk/ofxBeatTracking/blob/master/fft.cpp](here).

## Usage

Install the addon as usual, then in your openFrameworks app define an instance
of ofxBeat, for example:

    class testApp : public ofBaseApp{

      ofxBeat beat;

      // ... rest of your app class ...
    };

Then initialize the sound stream in your app `setup()` method, for example:

    void testApp::setup(){
      ofSoundStreamSetup(0, 1, this, 44100, beat.getBufferSize(), 4);
    }

Add a call to the update method to your app's update method to perform the
fft processing on recevied audio, for example:

    void testApp::update() {
      beat.update((*_state.sym)[REG_TIME]);
    }

You can then use the separate bands to get information in the current beat
graph. These methods return a value between 1 and 0. It's 1 when a beat
is detected and fades away to 0 based on the detected time between beats.

    float kick = beat.kick();
    float snare = beat.snare();
    float hihat = beat.hihat();

You can also access the raw fft bands using the following function, where `i`
is a value between 0 and 31 (i.e. 32 fft bands indexed from 0):

    float selectedBand = beat.getBand(i);

## Limitations

This is a pretty basic implementation of beat detection but it is fast and
gets a fairly good result.
