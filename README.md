# egui-video, a video playing library for [`egui`](https://github.com/emilk/egui)
[![crates.io](https://img.shields.io/crates/v/egui-video)](https://crates.io/crates/egui-video/0.1.0)
[![docs](https://docs.rs/egui-video/badge.svg)](https://docs.rs/egui-modal/0.1.0/egui_video/)
[![license](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/n00kii/egui-video/blob/main/README.md)

![no god please no](media/no_god.gif)

plays videos in egui from file path or from bytes

as of now, can't publish as a crate due to me depending on a modified version of `rust-ffmpeg`. until the [relevant 2 year old pr](https://github.com/zmwangx/rust-ffmpeg/pull/85) goes through, you have to specify this as a git dependancy in `Cargo.toml`

## usage:
```rust
/* called once (top level initialization) */

{ // if using audio...
    let audio_sys = sdl2::init()?.audio()?;
    let audio_device = egui_video::init_audio_device(&audio_sys)?;
    
    // don't let audio_device drop out of memory! (or else you lose audio)

    add_audio_device_to_state_somewhere(audio_device);
}
```
```rust
/* called once (creating a player) */

let mut player = Player::new(ctx, my_media_path)?;

{ // if using audio...
    player = player.with_audio(&mut my_state.audio_device)
}
```
```rust
/* called every frame (showing the player) */
player.ui(ui, [player.width as f32, player.height as f32]);
```
### current caveats
 - need to compile in `release` or `opt-level=3` otherwise limited playback performance
 - ~~bad (playback, seeking) performance with large resolution streams~~
 - ~~seeking can be slow (is there better way of dropping packets?)~~
 - ~~depending on the specific stream, seeking can fail and mess up playback/seekbar (something to do with dts?)~~
 - ~~no audio playback~~
