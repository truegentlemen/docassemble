---
layout: docs
title: Question Modifiers
short_title: Question Modifiers
---

There are a number of optional modifiers that can be included in
`question` blocks to control the appearance or behavior of the
question.

## `audio`

{% highlight yaml %}
---
question: Are you traveling to New York City?
yesno: going_to_nyc
audio: nyc_question.mp3
---
{% endhighlight %}

The `audio` modifier allows you to add audio to your questions.  An
audio player will appear above the question, and the user can press
play to hear the sound.  Uses [audio.js], which means that for maximum
portability you should always use mp3 audio files.

The filename can be constructed with [Mako].  A plain file path will
be assumed to point to a file in the `static` directory of the package
in which the [YAML] file resides.  A package reference may also be
included: e.g., `docassemble.demo:data/static/schumann-clip-3.mp3`.
A URL beginning with `http` or `https` may also be provided.

You can also play uploaded files:

{% highlight yaml %}
---
question: Please upload an MP3
fields:
  - MP3 file: user_mp3_file
    datatype: file
---
question: Let's listen to what you uploaded.
field: heard_music
audio: ${ user_mp3_file }
---
{% endhighlight %}

**docassemble** uses the [HTML5 audio tag] to allow users to play the
audio.  Not all browsers support every type of audio file.  In order
to make your audio files accessible to the greatest number of users,
you should include files in both `mp3` and `ogg` format.

For example, if your `audio` declaration points to a file, such as
`nyc_question.mp3`, then your interview package will contain a file
called `nyc_question.mp3` in the `data/static` directory.  If you also
include an OGG version of this audio file, called `nyc_question.ogg`,
in the same directory, then **docassemble** will make both files
available to the user, and the user's browser will use whichever file
works.  In your `audio` declaration, you can refer to either the `mp3`
or the `ogg` file.

Or, if your `mp3` and `ogg` alternatives are located in different
directories, you can do this:

{% highlight yaml %}
---
question: Are you traveling to New York City?
yesno: going_to_nyc
audio:
  - mp3/nyc_question.mp3
  - ogg/nyc_question.ogg
---
{% endhighlight %}

Or, if you are using hyperlinks to files on another server, you can
include different versions by doing something like this:

{% highlight yaml %}
---
question: Are you traveling to New York City?
yesno: going_to_nyc
audio:
  - http://example.com/files/audio/51/nyc_question.mp3
  - http://example.com/files/audio/23/nyc_question.ogg
---
{% endhighlight %}

If you refer to an uploaded file, **docassemble** will take care of
providing both `mp3` and `ogg` versions.  When users upload an audio
file, **docassemble** tries to convert it to the appropriate formats.
For this to work, ffmpeg and pacpl must be installed on your system.
Currently, **docassemble** can handle audio files uploaded in `mp3`,
`ogg`, `3gpp`, and `wav` formats.  If you need to be able to process
another type of audio file, **docassemble**'s source code can probably
be modified to support that audio type.

Note that there a number of limitations to playing audio in browsers.
For example, older Android devices will not play audio retrieved
through https, but will play the same audio retrieved through http.

## `video`

The `video` declaration is just like the `audio` declaration except that it displays a
video instead of an audio file.

{% highlight yaml %}
---
question: Are you traveling to New York City?
yesno: going_to_nyc
video: nyc_tourism.mp4
---
{% endhighlight %}

**docassemble** uses the [HTML5 video tag] to allow users to play the
audio.  Just as you should include both `mp3` and `ogg` audio files,
you should include both `mp4` and `ogv` video files, so that users of
many different browsers will all be able to see your videos.  These
are the two formats that the [HTML5 video tag] most widely supports.

If you refer to an uploaded video file, **docassemble** will take care
of providing both `mp4` and `ogv` versions.  When users upload a
video file, **docassemble** tries to convert it to the appropriate
formats.  For this to work, ffmpeg and pacpl must be installed on your
system.  Currently, **docassemble** can handle videos uploaded in
`mp4`, `ogv`, and `mov` formats.  If you need to be able to process
another type of video, **docassemble**'s source code can probably be
modified to support that video type.

You can also use the `video` declaration to embed [YouTube] and
[Vimeo] videos.  For example, if you want to embed a [YouTube] video
and the URL for the video is
`https://www.youtube.com/watch?v=RpgYyuLt7Dx` or
`https://youtu.be/RpgYyuLt7Dx`, you would write this:

{% highlight yaml %}
---
question: Are you traveling to New York City?
yesno: going_to_nyc
video: |
  [YOUTUBE RpgYyuLt7Dx]
---
{% endhighlight %}

If you want to embed a [Vimeo] video, the URL of which is
`https://vimeo.com/96044910`, you would write:

{% highlight yaml %}
---
question: Are you traveling to New York City?
yesno: going_to_nyc
video: |
  [VIMEO 96044910]
---
{% endhighlight %}

Note that you could not have written the above as `video:
[VIMEO 96044910]` -- that would have generated an error because [YAML]
thinks square brackets indicate a list of items, not plain text.  If
you want to write the declaration on one line, write `video:
"[VIMEO 96044910]"`.

## `help`

{% highlight yaml %}
---
question: How much money do you wish to seek in damages?
fields:
  - Money: damages_sought
    datatype: currency
help: |
  If you are not sure how much money to seek in damages, just ask
  for a million dollars, since you want ${ defendant } to suffer.
---
{% endhighlight %}

In the web app, users can use the navigation bar to toggle between the
"Question" tab and the "Help" tab.  The contents of the "Help" tab
consist of the contents of any `help` statements in the question being
presented, followed by the contents of any `interview help` blocks
contained within the interview.

You can add audio to your help text:

{% highlight yaml %}
---
question: How much money do you wish to seek in damages?
fields:
  - Money: damages_sought
    datatype: currency
help: |
  content: |
    If you are not sure how much money to seek in damages, just ask
    for a million dollars, since you want the defendant to suffer.
  audio: message_re_damages.mp3
---
{% endhighlight %}

You can also add video to help text using the `video` declaration.

## `decoration`

{% highlight yaml %}
---
question: Have you been saving your money?
yesno: user_has_saved_money
decoration: piggybank
---
{% endhighlight %}

The `decoration` modifier adds an icon to the right of the `question`
text.  In the example above, `piggybank` will need to have been
defined in an `image sets` or `images` block.

## `progress`

{% highlight yaml %}
---
question: Are you doing well?
yesno: user_is_well
progress: 50
---
{% endhighlight %}

If **docassemble** is configured to show a progress bar, the progress
bar will be set to 50% when this question is asked.

## `language`

{% highlight yaml %}
---
question: |
  What is the meaning of life?
fields:
  - Meaning of life: meaning_life
---
language: es
question: |
  ¿Cuál es el significado de la vida?
fields:
  - Significado de la Vida: meaning_life
---
{% endhighlight %}

**docassemble**'s [language support] allows a single interview to asks
questions different ways depending on the user's language.  You can
write questions in different languages that set the same variables.
**docassemble** will use whatever question matches the active
language.

The value of `language` must be a two-character lowercase [ISO-639-1]
code.  For example, Spanish is `es`, French is `fr`, and Arabic is `ar`.

For more information about how to set the active language, see
[language support].

Instead of explicitly setting a `language` for every question, you can
use `default language` to apply a particular language to the remaining
questions in the file (see [initial blocks]).

## `generic object`

{% highlight yaml %}
---
generic object: Individual
question: |
  So, ${ x.is_are_you() } a defendant in this case?
yesno: x.is_defendant
---
{% endhighlight %}

`generic object` is a very powerful feature in **docassemble** that
allows authors to express questions in general terms.

The above example will cause **docassemble** to ask "So, is John Smith
a defendant in this case?" if the interview logic calls for
`neighbor.is_defendant` and `neighbor` is an object of type
`Individual` whose name has been set to "John Smith."  Or, if the
interview logic calls for `user.is_defendant`, **docassemble** will
ask, "So, are you a defendant in this case?"

`x` is a special variable that should only be used in `generic object`
questions.  This question definition tells **docassemble** that if it
ever needs an `is_defendant` attribute for any object of type
`Individual`, it can get an answer by asking this question.

## `role`

{% highlight yaml %}
---
role: advocate
question: Is the client's explanation a sound one?
subquestion: |
  ${ client } proposed the following explanation:
  
  > ${ explanation }

  Is this a legally sufficient explanation?
yesno: explanation_is_sound
---
{% endhighlight %}

If your interview uses the [roles]({{ site.baseurl}}/docs/roles.html)
feature for multi-user interviews, the `role` modifier in a `question`
block will tell **docassemble** that if it ever tries to ask this
question, the user will need to have a particular role in order to
proceed.

`role` can be a list.
{% highlight yaml %}
role:
  - advocate
  - supervisor
{% endhighlight %}
In this case, the user's role can either "advocate" or "supervisor" in
order to be asked the question.

If the user does not have an appropriate role, **docassemble** will
look for a question in the interview in which `event` has been set to
`role_event`.

## `comment`

{% highlight yaml %}
---
question: Do you agree the weather is nice today? 
yesno: day_is_nice
comment: |
  We might wish to consider taking out this question.  It does not
  seem necessary.
---
{% endhighlight %}

To make a note to yourself about a question, which will not be seen by
the end user, you can use a `comment` statement.  It will be ignored
by **docassemble**, so it can contain any valid [YAML].

[YAML]: https://en.wikipedia.org/wiki/YAML
[initial blocks]: {{ site.baseurl }}/docs/initial.html
[language support]: {{ site.baseurl }}/docs/language.html
[ISO-639-1]: https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes
[audio.js]: http://kolber.github.io/audiojs/
[Mako]: http://www.makotemplates.org/
[HTML5 audio tag]: http://www.w3schools.com/html/html5_audio.asp
[HTML5 video tag]: http://www.w3schools.com/html/html5_video.asp
[YouTube]: https://www.youtube.com/
[Vimeo]: https://vimeo.com/