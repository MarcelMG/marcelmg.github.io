---
layout: post
title: Real-time speech keyword recognizer using a Convolutional Neural Network (CNN)
---
<!DOCTYPE html>
<html>
<head><meta charset="utf-8" />

<title>CNN_Speech_Keyword_Recognition</title>

<script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.1.10/require.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.0.3/jquery.min.js"></script>



<style type="text/css">
    /*!
*
* Twitter Bootstrap
*
*/
/*!
 * Bootstrap v3.3.7 (http://getbootstrap.com)
 * Copyright 2011-2016 Twitter, Inc.
 * Licensed under MIT (https://github.com/twbs/bootstrap/blob/master/LICENSE)
 */
/*! normalize.css v3.0.3 | MIT License | github.com/necolas/normalize.css */
html {
  font-family: sans-serif;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
}
body {
  margin: 0;
}
article,
aside,
details,
figcaption,
figure,
footer,
header,
hgroup,
main,
menu,
nav,
section,
summary {
  display: block;
}
audio,
canvas,
progress,
video {
  display: inline-block;
  vertical-align: baseline;
}
audio:not([controls]) {
  display: none;
  height: 0;
}
[hidden],
template {
  display: none;
}
a {
  background-color: transparent;
}
a:active,
a:hover {
  outline: 0;
}
abbr[title] {
  border-bottom: 1px dotted;
}
b,
strong {
  font-weight: bold;
}
dfn {
  font-style: italic;
}
h1 {
  font-size: 2em;
  margin: 0.67em 0;
}
mark {
  background: #ff0;
  color: #000;
}
small {
  font-size: 80%;
}
sub,
sup {
  font-size: 75%;
  line-height: 0;
  position: relative;
  vertical-align: baseline;
}
sup {
  top: -0.5em;
}
sub {
  bottom: -0.25em;
}
img {
  border: 0;
}
svg:not(:root) {
  overflow: hidden;
}
figure {
  margin: 1em 40px;
}
hr {
  box-sizing: content-box;
  height: 0;
}
pre {
  overflow: auto;
}
code,
kbd,
pre,
samp {
  font-family: monospace, monospace;
  font-size: 1em;
}
button,
input,
optgroup,
select,
textarea {
  color: inherit;
  font: inherit;
  margin: 0;
}
button {
  overflow: visible;
}
button,
select {
  text-transform: none;
}
button,
html input[type="button"],
input[type="reset"],
input[type="submit"] {
  -webkit-appearance: button;
  cursor: pointer;
}
button[disabled],
html input[disabled] {
  cursor: default;
}
button::-moz-focus-inner,
input::-moz-focus-inner {
  border: 0;
  padding: 0;
}
input {
  line-height: normal;
}
input[type="checkbox"],
input[type="radio"] {
  box-sizing: border-box;
  padding: 0;
}
input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  height: auto;
}
input[type="search"] {
  -webkit-appearance: textfield;
  box-sizing: content-box;
}
input[type="search"]::-webkit-search-cancel-button,
input[type="search"]::-webkit-search-decoration {
  -webkit-appearance: none;
}
fieldset {
  border: 1px solid #c0c0c0;
  margin: 0 2px;
  padding: 0.35em 0.625em 0.75em;
}
legend {
  border: 0;
  padding: 0;
}
textarea {
  overflow: auto;
}
optgroup {
  font-weight: bold;
}
table {
  border-collapse: collapse;
  border-spacing: 0;
}
td,
th {
  padding: 0;
}
/*! Source: https://github.com/h5bp/html5-boilerplate/blob/master/src/css/main.css */
@media print {
  *,
  *:before,
  *:after {
    background: transparent !important;
    box-shadow: none !important;
    text-shadow: none !important;
  }
  a,
  a:visited {
    text-decoration: underline;
  }
  a[href]:after {
    content: " (" attr(href) ")";
  }
  abbr[title]:after {
    content: " (" attr(title) ")";
  }
  a[href^="#"]:after,
  a[href^="javascript:"]:after {
    content: "";
  }
  pre,
  blockquote {
    border: 1px solid #999;
    page-break-inside: avoid;
  }
  thead {
    display: table-header-group;
  }
  tr,
  img {
    page-break-inside: avoid;
  }
  img {
    max-width: 100% !important;
  }
  p,
  h2,
  h3 {
    orphans: 3;
    widows: 3;
  }
  h2,
  h3 {
    page-break-after: avoid;
  }
  .navbar {
    display: none;
  }
  .btn > .caret,
  .dropup > .btn > .caret {
    border-top-color: #000 !important;
  }
  .label {
    border: 1px solid #000;
  }
  .table {
    border-collapse: collapse !important;
  }
  .table td,
  .table th {
    background-color: #fff !important;
  }
  .table-bordered th,
  .table-bordered td {
    border: 1px solid #ddd !important;
  }
}
@font-face {
  font-family: 'Glyphicons Halflings';
  src: url('../components/bootstrap/fonts/glyphicons-halflings-regular.eot');
  src: url('../components/bootstrap/fonts/glyphicons-halflings-regular.eot?#iefix') format('embedded-opentype'), url('../components/bootstrap/fonts/glyphicons-halflings-regular.woff2') format('woff2'), url('../components/bootstrap/fonts/glyphicons-halflings-regular.woff') format('woff'), url('../components/bootstrap/fonts/glyphicons-halflings-regular.ttf') format('truetype'), url('../components/bootstrap/fonts/glyphicons-halflings-regular.svg#glyphicons_halflingsregular') format('svg');
}
.glyphicon {
  position: relative;
  top: 1px;
  display: inline-block;
  font-family: 'Glyphicons Halflings';
  font-style: normal;
  font-weight: normal;
  line-height: 1;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.glyphicon-asterisk:before {
  content: "\002a";
}
.glyphicon-plus:before {
  content: "\002b";
}
.glyphicon-euro:before,
.glyphicon-eur:before {
  content: "\20ac";
}
.glyphicon-minus:before {
  content: "\2212";
}
.glyphicon-cloud:before {
  content: "\2601";
}
.glyphicon-envelope:before {
  content: "\2709";
}
.glyphicon-pencil:before {
  content: "\270f";
}
.glyphicon-glass:before {
  content: "\e001";
}
.glyphicon-music:before {
  content: "\e002";
}
.glyphicon-search:before {
  content: "\e003";
}
.glyphicon-heart:before {
  content: "\e005";
}
.glyphicon-star:before {
  content: "\e006";
}
.glyphicon-star-empty:before {
  content: "\e007";
}
.glyphicon-user:before {
  content: "\e008";
}
.glyphicon-film:before {
  content: "\e009";
}
.glyphicon-th-large:before {
  content: "\e010";
}
.glyphicon-th:before {
  content: "\e011";
}
.glyphicon-th-list:before {
  content: "\e012";
}
.glyphicon-ok:before {
  content: "\e013";
}
.glyphicon-remove:before {
  content: "\e014";
}
.glyphicon-zoom-in:before {
  content: "\e015";
}
.glyphicon-zoom-out:before {
  content: "\e016";
}
.glyphicon-off:before {
  content: "\e017";
}
.glyphicon-signal:before {
  content: "\e018";
}
.glyphicon-cog:before {
  content: "\e019";
}
.glyphicon-trash:before {
  content: "\e020";
}
.glyphicon-home:before {
  content: "\e021";
}
.glyphicon-file:before {
  content: "\e022";
}
.glyphicon-time:before {
  content: "\e023";
}
.glyphicon-road:before {
  content: "\e024";
}
.glyphicon-download-alt:before {
  content: "\e025";
}
.glyphicon-download:before {
  content: "\e026";
}
.glyphicon-upload:before {
  content: "\e027";
}
.glyphicon-inbox:before {
  content: "\e028";
}
.glyphicon-play-circle:before {
  content: "\e029";
}
.glyphicon-repeat:before {
  content: "\e030";
}
.glyphicon-refresh:before {
  content: "\e031";
}
.glyphicon-list-alt:before {
  content: "\e032";
}
.glyphicon-lock:before {
  content: "\e033";
}
.glyphicon-flag:before {
  content: "\e034";
}
.glyphicon-headphones:before {
  content: "\e035";
}
.glyphicon-volume-off:before {
  content: "\e036";
}
.glyphicon-volume-down:before {
  content: "\e037";
}
.glyphicon-volume-up:before {
  content: "\e038";
}
.glyphicon-qrcode:before {
  content: "\e039";
}
.glyphicon-barcode:before {
  content: "\e040";
}
.glyphicon-tag:before {
  content: "\e041";
}
.glyphicon-tags:before {
  content: "\e042";
}
.glyphicon-book:before {
  content: "\e043";
}
.glyphicon-bookmark:before {
  content: "\e044";
}
.glyphicon-print:before {
  content: "\e045";
}
.glyphicon-camera:before {
  content: "\e046";
}
.glyphicon-font:before {
  content: "\e047";
}
.glyphicon-bold:before {
  content: "\e048";
}
.glyphicon-italic:before {
  content: "\e049";
}
.glyphicon-text-height:before {
  content: "\e050";
}
.glyphicon-text-width:before {
  content: "\e051";
}
.glyphicon-align-left:before {
  content: "\e052";
}
.glyphicon-align-center:before {
  content: "\e053";
}
.glyphicon-align-right:before {
  content: "\e054";
}
.glyphicon-align-justify:before {
  content: "\e055";
}
.glyphicon-list:before {
  content: "\e056";
}
.glyphicon-indent-left:before {
  content: "\e057";
}
.glyphicon-indent-right:before {
  content: "\e058";
}
.glyphicon-facetime-video:before {
  content: "\e059";
}
.glyphicon-picture:before {
  content: "\e060";
}
.glyphicon-map-marker:before {
  content: "\e062";
}
.glyphicon-adjust:before {
  content: "\e063";
}
.glyphicon-tint:before {
  content: "\e064";
}
.glyphicon-edit:before {
  content: "\e065";
}
.glyphicon-share:before {
  content: "\e066";
}
.glyphicon-check:before {
  content: "\e067";
}
.glyphicon-move:before {
  content: "\e068";
}
.glyphicon-step-backward:before {
  content: "\e069";
}
.glyphicon-fast-backward:before {
  content: "\e070";
}
.glyphicon-backward:before {
  content: "\e071";
}
.glyphicon-play:before {
  content: "\e072";
}
.glyphicon-pause:before {
  content: "\e073";
}
.glyphicon-stop:before {
  content: "\e074";
}
.glyphicon-forward:before {
  content: "\e075";
}
.glyphicon-fast-forward:before {
  content: "\e076";
}
.glyphicon-step-forward:before {
  content: "\e077";
}
.glyphicon-eject:before {
  content: "\e078";
}
.glyphicon-chevron-left:before {
  content: "\e079";
}
.glyphicon-chevron-right:before {
  content: "\e080";
}
.glyphicon-plus-sign:before {
  content: "\e081";
}
.glyphicon-minus-sign:before {
  content: "\e082";
}
.glyphicon-remove-sign:before {
  content: "\e083";
}
.glyphicon-ok-sign:before {
  content: "\e084";
}
.glyphicon-question-sign:before {
  content: "\e085";
}
.glyphicon-info-sign:before {
  content: "\e086";
}
.glyphicon-screenshot:before {
  content: "\e087";
}
.glyphicon-remove-circle:before {
  content: "\e088";
}
.glyphicon-ok-circle:before {
  content: "\e089";
}
.glyphicon-ban-circle:before {
  content: "\e090";
}
.glyphicon-arrow-left:before {
  content: "\e091";
}
.glyphicon-arrow-right:before {
  content: "\e092";
}
.glyphicon-arrow-up:before {
  content: "\e093";
}
.glyphicon-arrow-down:before {
  content: "\e094";
}
.glyphicon-share-alt:before {
  content: "\e095";
}
.glyphicon-resize-full:before {
  content: "\e096";
}
.glyphicon-resize-small:before {
  content: "\e097";
}
.glyphicon-exclamation-sign:before {
  content: "\e101";
}
.glyphicon-gift:before {
  content: "\e102";
}
.glyphicon-leaf:before {
  content: "\e103";
}
.glyphicon-fire:before {
  content: "\e104";
}
.glyphicon-eye-open:before {
  content: "\e105";
}
.glyphicon-eye-close:before {
  content: "\e106";
}
.glyphicon-warning-sign:before {
  content: "\e107";
}
.glyphicon-plane:before {
  content: "\e108";
}
.glyphicon-calendar:before {
  content: "\e109";
}
.glyphicon-random:before {
  content: "\e110";
}
.glyphicon-comment:before {
  content: "\e111";
}
.glyphicon-magnet:before {
  content: "\e112";
}
.glyphicon-chevron-up:before {
  content: "\e113";
}
.glyphicon-chevron-down:before {
  content: "\e114";
}
.glyphicon-retweet:before {
  content: "\e115";
}
.glyphicon-shopping-cart:before {
  content: "\e116";
}
.glyphicon-folder-close:before {
  content: "\e117";
}
.glyphicon-folder-open:before {
  content: "\e118";
}
.glyphicon-resize-vertical:before {
  content: "\e119";
}
.glyphicon-resize-horizontal:before {
  content: "\e120";
}
.glyphicon-hdd:before {
  content: "\e121";
}
.glyphicon-bullhorn:before {
  content: "\e122";
}
.glyphicon-bell:before {
  content: "\e123";
}
.glyphicon-certificate:before {
  content: "\e124";
}
.glyphicon-thumbs-up:before {
  content: "\e125";
}
.glyphicon-thumbs-down:before {
  content: "\e126";
}
.glyphicon-hand-right:before {
  content: "\e127";
}
.glyphicon-hand-left:before {
  content: "\e128";
}
.glyphicon-hand-up:before {
  content: "\e129";
}
.glyphicon-hand-down:before {
  content: "\e130";
}
.glyphicon-circle-arrow-right:before {
  content: "\e131";
}
.glyphicon-circle-arrow-left:before {
  content: "\e132";
}
.glyphicon-circle-arrow-up:before {
  content: "\e133";
}
.glyphicon-circle-arrow-down:before {
  content: "\e134";
}
.glyphicon-globe:before {
  content: "\e135";
}
.glyphicon-wrench:before {
  content: "\e136";
}
.glyphicon-tasks:before {
  content: "\e137";
}
.glyphicon-filter:before {
  content: "\e138";
}
.glyphicon-briefcase:before {
  content: "\e139";
}
.glyphicon-fullscreen:before {
  content: "\e140";
}
.glyphicon-dashboard:before {
  content: "\e141";
}
.glyphicon-paperclip:before {
  content: "\e142";
}
.glyphicon-heart-empty:before {
  content: "\e143";
}
.glyphicon-link:before {
  content: "\e144";
}
.glyphicon-phone:before {
  content: "\e145";
}
.glyphicon-pushpin:before {
  content: "\e146";
}
.glyphicon-usd:before {
  content: "\e148";
}
.glyphicon-gbp:before {
  content: "\e149";
}
.glyphicon-sort:before {
  content: "\e150";
}
.glyphicon-sort-by-alphabet:before {
  content: "\e151";
}
.glyphicon-sort-by-alphabet-alt:before {
  content: "\e152";
}
.glyphicon-sort-by-order:before {
  content: "\e153";
}
.glyphicon-sort-by-order-alt:before {
  content: "\e154";
}
.glyphicon-sort-by-attributes:before {
  content: "\e155";
}
.glyphicon-sort-by-attributes-alt:before {
  content: "\e156";
}
.glyphicon-unchecked:before {
  content: "\e157";
}
.glyphicon-expand:before {
  content: "\e158";
}
.glyphicon-collapse-down:before {
  content: "\e159";
}
.glyphicon-collapse-up:before {
  content: "\e160";
}
.glyphicon-log-in:before {
  content: "\e161";
}
.glyphicon-flash:before {
  content: "\e162";
}
.glyphicon-log-out:before {
  content: "\e163";
}
.glyphicon-new-window:before {
  content: "\e164";
}
.glyphicon-record:before {
  content: "\e165";
}
.glyphicon-save:before {
  content: "\e166";
}
.glyphicon-open:before {
  content: "\e167";
}
.glyphicon-saved:before {
  content: "\e168";
}
.glyphicon-import:before {
  content: "\e169";
}
.glyphicon-export:before {
  content: "\e170";
}
.glyphicon-send:before {
  content: "\e171";
}
.glyphicon-floppy-disk:before {
  content: "\e172";
}
.glyphicon-floppy-saved:before {
  content: "\e173";
}
.glyphicon-floppy-remove:before {
  content: "\e174";
}
.glyphicon-floppy-save:before {
  content: "\e175";
}
.glyphicon-floppy-open:before {
  content: "\e176";
}
.glyphicon-credit-card:before {
  content: "\e177";
}
.glyphicon-transfer:before {
  content: "\e178";
}
.glyphicon-cutlery:before {
  content: "\e179";
}
.glyphicon-header:before {
  content: "\e180";
}
.glyphicon-compressed:before {
  content: "\e181";
}
.glyphicon-earphone:before {
  content: "\e182";
}
.glyphicon-phone-alt:before {
  content: "\e183";
}
.glyphicon-tower:before {
  content: "\e184";
}
.glyphicon-stats:before {
  content: "\e185";
}
.glyphicon-sd-video:before {
  content: "\e186";
}
.glyphicon-hd-video:before {
  content: "\e187";
}
.glyphicon-subtitles:before {
  content: "\e188";
}
.glyphicon-sound-stereo:before {
  content: "\e189";
}
.glyphicon-sound-dolby:before {
  content: "\e190";
}
.glyphicon-sound-5-1:before {
  content: "\e191";
}
.glyphicon-sound-6-1:before {
  content: "\e192";
}
.glyphicon-sound-7-1:before {
  content: "\e193";
}
.glyphicon-copyright-mark:before {
  content: "\e194";
}
.glyphicon-registration-mark:before {
  content: "\e195";
}
.glyphicon-cloud-download:before {
  content: "\e197";
}
.glyphicon-cloud-upload:before {
  content: "\e198";
}
.glyphicon-tree-conifer:before {
  content: "\e199";
}
.glyphicon-tree-deciduous:before {
  content: "\e200";
}
.glyphicon-cd:before {
  content: "\e201";
}
.glyphicon-save-file:before {
  content: "\e202";
}
.glyphicon-open-file:before {
  content: "\e203";
}
.glyphicon-level-up:before {
  content: "\e204";
}
.glyphicon-copy:before {
  content: "\e205";
}
.glyphicon-paste:before {
  content: "\e206";
}
.glyphicon-alert:before {
  content: "\e209";
}
.glyphicon-equalizer:before {
  content: "\e210";
}
.glyphicon-king:before {
  content: "\e211";
}
.glyphicon-queen:before {
  content: "\e212";
}
.glyphicon-pawn:before {
  content: "\e213";
}
.glyphicon-bishop:before {
  content: "\e214";
}
.glyphicon-knight:before {
  content: "\e215";
}
.glyphicon-baby-formula:before {
  content: "\e216";
}
.glyphicon-tent:before {
  content: "\26fa";
}
.glyphicon-blackboard:before {
  content: "\e218";
}
.glyphicon-bed:before {
  content: "\e219";
}
.glyphicon-apple:before {
  content: "\f8ff";
}
.glyphicon-erase:before {
  content: "\e221";
}
.glyphicon-hourglass:before {
  content: "\231b";
}
.glyphicon-lamp:before {
  content: "\e223";
}
.glyphicon-duplicate:before {
  content: "\e224";
}
.glyphicon-piggy-bank:before {
  content: "\e225";
}
.glyphicon-scissors:before {
  content: "\e226";
}
.glyphicon-bitcoin:before {
  content: "\e227";
}
.glyphicon-btc:before {
  content: "\e227";
}
.glyphicon-xbt:before {
  content: "\e227";
}
.glyphicon-yen:before {
  content: "\00a5";
}
.glyphicon-jpy:before {
  content: "\00a5";
}
.glyphicon-ruble:before {
  content: "\20bd";
}
.glyphicon-rub:before {
  content: "\20bd";
}
.glyphicon-scale:before {
  content: "\e230";
}
.glyphicon-ice-lolly:before {
  content: "\e231";
}
.glyphicon-ice-lolly-tasted:before {
  content: "\e232";
}
.glyphicon-education:before {
  content: "\e233";
}
.glyphicon-option-horizontal:before {
  content: "\e234";
}
.glyphicon-option-vertical:before {
  content: "\e235";
}
.glyphicon-menu-hamburger:before {
  content: "\e236";
}
.glyphicon-modal-window:before {
  content: "\e237";
}
.glyphicon-oil:before {
  content: "\e238";
}
.glyphicon-grain:before {
  content: "\e239";
}
.glyphicon-sunglasses:before {
  content: "\e240";
}
.glyphicon-text-size:before {
  content: "\e241";
}
.glyphicon-text-color:before {
  content: "\e242";
}
.glyphicon-text-background:before {
  content: "\e243";
}
.glyphicon-object-align-top:before {
  content: "\e244";
}
.glyphicon-object-align-bottom:before {
  content: "\e245";
}
.glyphicon-object-align-horizontal:before {
  content: "\e246";
}
.glyphicon-object-align-left:before {
  content: "\e247";
}
.glyphicon-object-align-vertical:before {
  content: "\e248";
}
.glyphicon-object-align-right:before {
  content: "\e249";
}
.glyphicon-triangle-right:before {
  content: "\e250";
}
.glyphicon-triangle-left:before {
  content: "\e251";
}
.glyphicon-triangle-bottom:before {
  content: "\e252";
}
.glyphicon-triangle-top:before {
  content: "\e253";
}
.glyphicon-console:before {
  content: "\e254";
}
.glyphicon-superscript:before {
  content: "\e255";
}
.glyphicon-subscript:before {
  content: "\e256";
}
.glyphicon-menu-left:before {
  content: "\e257";
}
.glyphicon-menu-right:before {
  content: "\e258";
}
.glyphicon-menu-down:before {
  content: "\e259";
}
.glyphicon-menu-up:before {
  content: "\e260";
}
* {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
*:before,
*:after {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
html {
  font-size: 10px;
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}
body {
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-size: 13px;
  line-height: 1.42857143;
  color: #000;
  background-color: #fff;
}
input,
button,
select,
textarea {
  font-family: inherit;
  font-size: inherit;
  line-height: inherit;
}
a {
  color: #337ab7;
  text-decoration: none;
}
a:hover,
a:focus {
  color: #23527c;
  text-decoration: underline;
}
a:focus {
  outline: 5px auto -webkit-focus-ring-color;
  outline-offset: -2px;
}
figure {
  margin: 0;
}
img {
  vertical-align: middle;
}
.img-responsive,
.thumbnail > img,
.thumbnail a > img,
.carousel-inner > .item > img,
.carousel-inner > .item > a > img {
  display: block;
  max-width: 100%;
  height: auto;
}
.img-rounded {
  border-radius: 3px;
}
.img-thumbnail {
  padding: 4px;
  line-height: 1.42857143;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 2px;
  -webkit-transition: all 0.2s ease-in-out;
  -o-transition: all 0.2s ease-in-out;
  transition: all 0.2s ease-in-out;
  display: inline-block;
  max-width: 100%;
  height: auto;
}
.img-circle {
  border-radius: 50%;
}
hr {
  margin-top: 18px;
  margin-bottom: 18px;
  border: 0;
  border-top: 1px solid #eeeeee;
}
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  padding: 0;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  border: 0;
}
.sr-only-focusable:active,
.sr-only-focusable:focus {
  position: static;
  width: auto;
  height: auto;
  margin: 0;
  overflow: visible;
  clip: auto;
}
[role="button"] {
  cursor: pointer;
}
h1,
h2,
h3,
h4,
h5,
h6,
.h1,
.h2,
.h3,
.h4,
.h5,
.h6 {
  font-family: inherit;
  font-weight: 500;
  line-height: 1.1;
  color: inherit;
}
h1 small,
h2 small,
h3 small,
h4 small,
h5 small,
h6 small,
.h1 small,
.h2 small,
.h3 small,
.h4 small,
.h5 small,
.h6 small,
h1 .small,
h2 .small,
h3 .small,
h4 .small,
h5 .small,
h6 .small,
.h1 .small,
.h2 .small,
.h3 .small,
.h4 .small,
.h5 .small,
.h6 .small {
  font-weight: normal;
  line-height: 1;
  color: #777777;
}
h1,
.h1,
h2,
.h2,
h3,
.h3 {
  margin-top: 18px;
  margin-bottom: 9px;
}
h1 small,
.h1 small,
h2 small,
.h2 small,
h3 small,
.h3 small,
h1 .small,
.h1 .small,
h2 .small,
.h2 .small,
h3 .small,
.h3 .small {
  font-size: 65%;
}
h4,
.h4,
h5,
.h5,
h6,
.h6 {
  margin-top: 9px;
  margin-bottom: 9px;
}
h4 small,
.h4 small,
h5 small,
.h5 small,
h6 small,
.h6 small,
h4 .small,
.h4 .small,
h5 .small,
.h5 .small,
h6 .small,
.h6 .small {
  font-size: 75%;
}
h1,
.h1 {
  font-size: 33px;
}
h2,
.h2 {
  font-size: 27px;
}
h3,
.h3 {
  font-size: 23px;
}
h4,
.h4 {
  font-size: 17px;
}
h5,
.h5 {
  font-size: 13px;
}
h6,
.h6 {
  font-size: 12px;
}
p {
  margin: 0 0 9px;
}
.lead {
  margin-bottom: 18px;
  font-size: 14px;
  font-weight: 300;
  line-height: 1.4;
}
@media (min-width: 768px) {
  .lead {
    font-size: 19.5px;
  }
}
small,
.small {
  font-size: 92%;
}
mark,
.mark {
  background-color: #fcf8e3;
  padding: .2em;
}
.text-left {
  text-align: left;
}
.text-right {
  text-align: right;
}
.text-center {
  text-align: center;
}
.text-justify {
  text-align: justify;
}
.text-nowrap {
  white-space: nowrap;
}
.text-lowercase {
  text-transform: lowercase;
}
.text-uppercase {
  text-transform: uppercase;
}
.text-capitalize {
  text-transform: capitalize;
}
.text-muted {
  color: #777777;
}
.text-primary {
  color: #337ab7;
}
a.text-primary:hover,
a.text-primary:focus {
  color: #286090;
}
.text-success {
  color: #3c763d;
}
a.text-success:hover,
a.text-success:focus {
  color: #2b542c;
}
.text-info {
  color: #31708f;
}
a.text-info:hover,
a.text-info:focus {
  color: #245269;
}
.text-warning {
  color: #8a6d3b;
}
a.text-warning:hover,
a.text-warning:focus {
  color: #66512c;
}
.text-danger {
  color: #a94442;
}
a.text-danger:hover,
a.text-danger:focus {
  color: #843534;
}
.bg-primary {
  color: #fff;
  background-color: #337ab7;
}
a.bg-primary:hover,
a.bg-primary:focus {
  background-color: #286090;
}
.bg-success {
  background-color: #dff0d8;
}
a.bg-success:hover,
a.bg-success:focus {
  background-color: #c1e2b3;
}
.bg-info {
  background-color: #d9edf7;
}
a.bg-info:hover,
a.bg-info:focus {
  background-color: #afd9ee;
}
.bg-warning {
  background-color: #fcf8e3;
}
a.bg-warning:hover,
a.bg-warning:focus {
  background-color: #f7ecb5;
}
.bg-danger {
  background-color: #f2dede;
}
a.bg-danger:hover,
a.bg-danger:focus {
  background-color: #e4b9b9;
}
.page-header {
  padding-bottom: 8px;
  margin: 36px 0 18px;
  border-bottom: 1px solid #eeeeee;
}
ul,
ol {
  margin-top: 0;
  margin-bottom: 9px;
}
ul ul,
ol ul,
ul ol,
ol ol {
  margin-bottom: 0;
}
.list-unstyled {
  padding-left: 0;
  list-style: none;
}
.list-inline {
  padding-left: 0;
  list-style: none;
  margin-left: -5px;
}
.list-inline > li {
  display: inline-block;
  padding-left: 5px;
  padding-right: 5px;
}
dl {
  margin-top: 0;
  margin-bottom: 18px;
}
dt,
dd {
  line-height: 1.42857143;
}
dt {
  font-weight: bold;
}
dd {
  margin-left: 0;
}
@media (min-width: 541px) {
  .dl-horizontal dt {
    float: left;
    width: 160px;
    clear: left;
    text-align: right;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  .dl-horizontal dd {
    margin-left: 180px;
  }
}
abbr[title],
abbr[data-original-title] {
  cursor: help;
  border-bottom: 1px dotted #777777;
}
.initialism {
  font-size: 90%;
  text-transform: uppercase;
}
blockquote {
  padding: 9px 18px;
  margin: 0 0 18px;
  font-size: inherit;
  border-left: 5px solid #eeeeee;
}
blockquote p:last-child,
blockquote ul:last-child,
blockquote ol:last-child {
  margin-bottom: 0;
}
blockquote footer,
blockquote small,
blockquote .small {
  display: block;
  font-size: 80%;
  line-height: 1.42857143;
  color: #777777;
}
blockquote footer:before,
blockquote small:before,
blockquote .small:before {
  content: '\2014 \00A0';
}
.blockquote-reverse,
blockquote.pull-right {
  padding-right: 15px;
  padding-left: 0;
  border-right: 5px solid #eeeeee;
  border-left: 0;
  text-align: right;
}
.blockquote-reverse footer:before,
blockquote.pull-right footer:before,
.blockquote-reverse small:before,
blockquote.pull-right small:before,
.blockquote-reverse .small:before,
blockquote.pull-right .small:before {
  content: '';
}
.blockquote-reverse footer:after,
blockquote.pull-right footer:after,
.blockquote-reverse small:after,
blockquote.pull-right small:after,
.blockquote-reverse .small:after,
blockquote.pull-right .small:after {
  content: '\00A0 \2014';
}
address {
  margin-bottom: 18px;
  font-style: normal;
  line-height: 1.42857143;
}
code,
kbd,
pre,
samp {
  font-family: monospace;
}
code {
  padding: 2px 4px;
  font-size: 90%;
  color: #c7254e;
  background-color: #f9f2f4;
  border-radius: 2px;
}
kbd {
  padding: 2px 4px;
  font-size: 90%;
  color: #888;
  background-color: transparent;
  border-radius: 1px;
  box-shadow: inset 0 -1px 0 rgba(0, 0, 0, 0.25);
}
kbd kbd {
  padding: 0;
  font-size: 100%;
  font-weight: bold;
  box-shadow: none;
}
pre {
  display: block;
  padding: 8.5px;
  margin: 0 0 9px;
  font-size: 12px;
  line-height: 1.42857143;
  word-break: break-all;
  word-wrap: break-word;
  color: #333333;
  background-color: #f5f5f5;
  border: 1px solid #ccc;
  border-radius: 2px;
}
pre code {
  padding: 0;
  font-size: inherit;
  color: inherit;
  white-space: pre-wrap;
  background-color: transparent;
  border-radius: 0;
}
.pre-scrollable {
  max-height: 340px;
  overflow-y: scroll;
}
.container {
  margin-right: auto;
  margin-left: auto;
  padding-left: 0px;
  padding-right: 0px;
}
@media (min-width: 768px) {
  .container {
    width: 768px;
  }
}
@media (min-width: 992px) {
  .container {
    width: 940px;
  }
}
@media (min-width: 1200px) {
  .container {
    width: 1140px;
  }
}
.container-fluid {
  margin-right: auto;
  margin-left: auto;
  padding-left: 0px;
  padding-right: 0px;
}
.row {
  margin-left: 0px;
  margin-right: 0px;
}
.col-xs-1, .col-sm-1, .col-md-1, .col-lg-1, .col-xs-2, .col-sm-2, .col-md-2, .col-lg-2, .col-xs-3, .col-sm-3, .col-md-3, .col-lg-3, .col-xs-4, .col-sm-4, .col-md-4, .col-lg-4, .col-xs-5, .col-sm-5, .col-md-5, .col-lg-5, .col-xs-6, .col-sm-6, .col-md-6, .col-lg-6, .col-xs-7, .col-sm-7, .col-md-7, .col-lg-7, .col-xs-8, .col-sm-8, .col-md-8, .col-lg-8, .col-xs-9, .col-sm-9, .col-md-9, .col-lg-9, .col-xs-10, .col-sm-10, .col-md-10, .col-lg-10, .col-xs-11, .col-sm-11, .col-md-11, .col-lg-11, .col-xs-12, .col-sm-12, .col-md-12, .col-lg-12 {
  position: relative;
  min-height: 1px;
  padding-left: 0px;
  padding-right: 0px;
}
.col-xs-1, .col-xs-2, .col-xs-3, .col-xs-4, .col-xs-5, .col-xs-6, .col-xs-7, .col-xs-8, .col-xs-9, .col-xs-10, .col-xs-11, .col-xs-12 {
  float: left;
}
.col-xs-12 {
  width: 100%;
}
.col-xs-11 {
  width: 91.66666667%;
}
.col-xs-10 {
  width: 83.33333333%;
}
.col-xs-9 {
  width: 75%;
}
.col-xs-8 {
  width: 66.66666667%;
}
.col-xs-7 {
  width: 58.33333333%;
}
.col-xs-6 {
  width: 50%;
}
.col-xs-5 {
  width: 41.66666667%;
}
.col-xs-4 {
  width: 33.33333333%;
}
.col-xs-3 {
  width: 25%;
}
.col-xs-2 {
  width: 16.66666667%;
}
.col-xs-1 {
  width: 8.33333333%;
}
.col-xs-pull-12 {
  right: 100%;
}
.col-xs-pull-11 {
  right: 91.66666667%;
}
.col-xs-pull-10 {
  right: 83.33333333%;
}
.col-xs-pull-9 {
  right: 75%;
}
.col-xs-pull-8 {
  right: 66.66666667%;
}
.col-xs-pull-7 {
  right: 58.33333333%;
}
.col-xs-pull-6 {
  right: 50%;
}
.col-xs-pull-5 {
  right: 41.66666667%;
}
.col-xs-pull-4 {
  right: 33.33333333%;
}
.col-xs-pull-3 {
  right: 25%;
}
.col-xs-pull-2 {
  right: 16.66666667%;
}
.col-xs-pull-1 {
  right: 8.33333333%;
}
.col-xs-pull-0 {
  right: auto;
}
.col-xs-push-12 {
  left: 100%;
}
.col-xs-push-11 {
  left: 91.66666667%;
}
.col-xs-push-10 {
  left: 83.33333333%;
}
.col-xs-push-9 {
  left: 75%;
}
.col-xs-push-8 {
  left: 66.66666667%;
}
.col-xs-push-7 {
  left: 58.33333333%;
}
.col-xs-push-6 {
  left: 50%;
}
.col-xs-push-5 {
  left: 41.66666667%;
}
.col-xs-push-4 {
  left: 33.33333333%;
}
.col-xs-push-3 {
  left: 25%;
}
.col-xs-push-2 {
  left: 16.66666667%;
}
.col-xs-push-1 {
  left: 8.33333333%;
}
.col-xs-push-0 {
  left: auto;
}
.col-xs-offset-12 {
  margin-left: 100%;
}
.col-xs-offset-11 {
  margin-left: 91.66666667%;
}
.col-xs-offset-10 {
  margin-left: 83.33333333%;
}
.col-xs-offset-9 {
  margin-left: 75%;
}
.col-xs-offset-8 {
  margin-left: 66.66666667%;
}
.col-xs-offset-7 {
  margin-left: 58.33333333%;
}
.col-xs-offset-6 {
  margin-left: 50%;
}
.col-xs-offset-5 {
  margin-left: 41.66666667%;
}
.col-xs-offset-4 {
  margin-left: 33.33333333%;
}
.col-xs-offset-3 {
  margin-left: 25%;
}
.col-xs-offset-2 {
  margin-left: 16.66666667%;
}
.col-xs-offset-1 {
  margin-left: 8.33333333%;
}
.col-xs-offset-0 {
  margin-left: 0%;
}
@media (min-width: 768px) {
  .col-sm-1, .col-sm-2, .col-sm-3, .col-sm-4, .col-sm-5, .col-sm-6, .col-sm-7, .col-sm-8, .col-sm-9, .col-sm-10, .col-sm-11, .col-sm-12 {
    float: left;
  }
  .col-sm-12 {
    width: 100%;
  }
  .col-sm-11 {
    width: 91.66666667%;
  }
  .col-sm-10 {
    width: 83.33333333%;
  }
  .col-sm-9 {
    width: 75%;
  }
  .col-sm-8 {
    width: 66.66666667%;
  }
  .col-sm-7 {
    width: 58.33333333%;
  }
  .col-sm-6 {
    width: 50%;
  }
  .col-sm-5 {
    width: 41.66666667%;
  }
  .col-sm-4 {
    width: 33.33333333%;
  }
  .col-sm-3 {
    width: 25%;
  }
  .col-sm-2 {
    width: 16.66666667%;
  }
  .col-sm-1 {
    width: 8.33333333%;
  }
  .col-sm-pull-12 {
    right: 100%;
  }
  .col-sm-pull-11 {
    right: 91.66666667%;
  }
  .col-sm-pull-10 {
    right: 83.33333333%;
  }
  .col-sm-pull-9 {
    right: 75%;
  }
  .col-sm-pull-8 {
    right: 66.66666667%;
  }
  .col-sm-pull-7 {
    right: 58.33333333%;
  }
  .col-sm-pull-6 {
    right: 50%;
  }
  .col-sm-pull-5 {
    right: 41.66666667%;
  }
  .col-sm-pull-4 {
    right: 33.33333333%;
  }
  .col-sm-pull-3 {
    right: 25%;
  }
  .col-sm-pull-2 {
    right: 16.66666667%;
  }
  .col-sm-pull-1 {
    right: 8.33333333%;
  }
  .col-sm-pull-0 {
    right: auto;
  }
  .col-sm-push-12 {
    left: 100%;
  }
  .col-sm-push-11 {
    left: 91.66666667%;
  }
  .col-sm-push-10 {
    left: 83.33333333%;
  }
  .col-sm-push-9 {
    left: 75%;
  }
  .col-sm-push-8 {
    left: 66.66666667%;
  }
  .col-sm-push-7 {
    left: 58.33333333%;
  }
  .col-sm-push-6 {
    left: 50%;
  }
  .col-sm-push-5 {
    left: 41.66666667%;
  }
  .col-sm-push-4 {
    left: 33.33333333%;
  }
  .col-sm-push-3 {
    left: 25%;
  }
  .col-sm-push-2 {
    left: 16.66666667%;
  }
  .col-sm-push-1 {
    left: 8.33333333%;
  }
  .col-sm-push-0 {
    left: auto;
  }
  .col-sm-offset-12 {
    margin-left: 100%;
  }
  .col-sm-offset-11 {
    margin-left: 91.66666667%;
  }
  .col-sm-offset-10 {
    margin-left: 83.33333333%;
  }
  .col-sm-offset-9 {
    margin-left: 75%;
  }
  .col-sm-offset-8 {
    margin-left: 66.66666667%;
  }
  .col-sm-offset-7 {
    margin-left: 58.33333333%;
  }
  .col-sm-offset-6 {
    margin-left: 50%;
  }
  .col-sm-offset-5 {
    margin-left: 41.66666667%;
  }
  .col-sm-offset-4 {
    margin-left: 33.33333333%;
  }
  .col-sm-offset-3 {
    margin-left: 25%;
  }
  .col-sm-offset-2 {
    margin-left: 16.66666667%;
  }
  .col-sm-offset-1 {
    margin-left: 8.33333333%;
  }
  .col-sm-offset-0 {
    margin-left: 0%;
  }
}
@media (min-width: 992px) {
  .col-md-1, .col-md-2, .col-md-3, .col-md-4, .col-md-5, .col-md-6, .col-md-7, .col-md-8, .col-md-9, .col-md-10, .col-md-11, .col-md-12 {
    float: left;
  }
  .col-md-12 {
    width: 100%;
  }
  .col-md-11 {
    width: 91.66666667%;
  }
  .col-md-10 {
    width: 83.33333333%;
  }
  .col-md-9 {
    width: 75%;
  }
  .col-md-8 {
    width: 66.66666667%;
  }
  .col-md-7 {
    width: 58.33333333%;
  }
  .col-md-6 {
    width: 50%;
  }
  .col-md-5 {
    width: 41.66666667%;
  }
  .col-md-4 {
    width: 33.33333333%;
  }
  .col-md-3 {
    width: 25%;
  }
  .col-md-2 {
    width: 16.66666667%;
  }
  .col-md-1 {
    width: 8.33333333%;
  }
  .col-md-pull-12 {
    right: 100%;
  }
  .col-md-pull-11 {
    right: 91.66666667%;
  }
  .col-md-pull-10 {
    right: 83.33333333%;
  }
  .col-md-pull-9 {
    right: 75%;
  }
  .col-md-pull-8 {
    right: 66.66666667%;
  }
  .col-md-pull-7 {
    right: 58.33333333%;
  }
  .col-md-pull-6 {
    right: 50%;
  }
  .col-md-pull-5 {
    right: 41.66666667%;
  }
  .col-md-pull-4 {
    right: 33.33333333%;
  }
  .col-md-pull-3 {
    right: 25%;
  }
  .col-md-pull-2 {
    right: 16.66666667%;
  }
  .col-md-pull-1 {
    right: 8.33333333%;
  }
  .col-md-pull-0 {
    right: auto;
  }
  .col-md-push-12 {
    left: 100%;
  }
  .col-md-push-11 {
    left: 91.66666667%;
  }
  .col-md-push-10 {
    left: 83.33333333%;
  }
  .col-md-push-9 {
    left: 75%;
  }
  .col-md-push-8 {
    left: 66.66666667%;
  }
  .col-md-push-7 {
    left: 58.33333333%;
  }
  .col-md-push-6 {
    left: 50%;
  }
  .col-md-push-5 {
    left: 41.66666667%;
  }
  .col-md-push-4 {
    left: 33.33333333%;
  }
  .col-md-push-3 {
    left: 25%;
  }
  .col-md-push-2 {
    left: 16.66666667%;
  }
  .col-md-push-1 {
    left: 8.33333333%;
  }
  .col-md-push-0 {
    left: auto;
  }
  .col-md-offset-12 {
    margin-left: 100%;
  }
  .col-md-offset-11 {
    margin-left: 91.66666667%;
  }
  .col-md-offset-10 {
    margin-left: 83.33333333%;
  }
  .col-md-offset-9 {
    margin-left: 75%;
  }
  .col-md-offset-8 {
    margin-left: 66.66666667%;
  }
  .col-md-offset-7 {
    margin-left: 58.33333333%;
  }
  .col-md-offset-6 {
    margin-left: 50%;
  }
  .col-md-offset-5 {
    margin-left: 41.66666667%;
  }
  .col-md-offset-4 {
    margin-left: 33.33333333%;
  }
  .col-md-offset-3 {
    margin-left: 25%;
  }
  .col-md-offset-2 {
    margin-left: 16.66666667%;
  }
  .col-md-offset-1 {
    margin-left: 8.33333333%;
  }
  .col-md-offset-0 {
    margin-left: 0%;
  }
}
@media (min-width: 1200px) {
  .col-lg-1, .col-lg-2, .col-lg-3, .col-lg-4, .col-lg-5, .col-lg-6, .col-lg-7, .col-lg-8, .col-lg-9, .col-lg-10, .col-lg-11, .col-lg-12 {
    float: left;
  }
  .col-lg-12 {
    width: 100%;
  }
  .col-lg-11 {
    width: 91.66666667%;
  }
  .col-lg-10 {
    width: 83.33333333%;
  }
  .col-lg-9 {
    width: 75%;
  }
  .col-lg-8 {
    width: 66.66666667%;
  }
  .col-lg-7 {
    width: 58.33333333%;
  }
  .col-lg-6 {
    width: 50%;
  }
  .col-lg-5 {
    width: 41.66666667%;
  }
  .col-lg-4 {
    width: 33.33333333%;
  }
  .col-lg-3 {
    width: 25%;
  }
  .col-lg-2 {
    width: 16.66666667%;
  }
  .col-lg-1 {
    width: 8.33333333%;
  }
  .col-lg-pull-12 {
    right: 100%;
  }
  .col-lg-pull-11 {
    right: 91.66666667%;
  }
  .col-lg-pull-10 {
    right: 83.33333333%;
  }
  .col-lg-pull-9 {
    right: 75%;
  }
  .col-lg-pull-8 {
    right: 66.66666667%;
  }
  .col-lg-pull-7 {
    right: 58.33333333%;
  }
  .col-lg-pull-6 {
    right: 50%;
  }
  .col-lg-pull-5 {
    right: 41.66666667%;
  }
  .col-lg-pull-4 {
    right: 33.33333333%;
  }
  .col-lg-pull-3 {
    right: 25%;
  }
  .col-lg-pull-2 {
    right: 16.66666667%;
  }
  .col-lg-pull-1 {
    right: 8.33333333%;
  }
  .col-lg-pull-0 {
    right: auto;
  }
  .col-lg-push-12 {
    left: 100%;
  }
  .col-lg-push-11 {
    left: 91.66666667%;
  }
  .col-lg-push-10 {
    left: 83.33333333%;
  }
  .col-lg-push-9 {
    left: 75%;
  }
  .col-lg-push-8 {
    left: 66.66666667%;
  }
  .col-lg-push-7 {
    left: 58.33333333%;
  }
  .col-lg-push-6 {
    left: 50%;
  }
  .col-lg-push-5 {
    left: 41.66666667%;
  }
  .col-lg-push-4 {
    left: 33.33333333%;
  }
  .col-lg-push-3 {
    left: 25%;
  }
  .col-lg-push-2 {
    left: 16.66666667%;
  }
  .col-lg-push-1 {
    left: 8.33333333%;
  }
  .col-lg-push-0 {
    left: auto;
  }
  .col-lg-offset-12 {
    margin-left: 100%;
  }
  .col-lg-offset-11 {
    margin-left: 91.66666667%;
  }
  .col-lg-offset-10 {
    margin-left: 83.33333333%;
  }
  .col-lg-offset-9 {
    margin-left: 75%;
  }
  .col-lg-offset-8 {
    margin-left: 66.66666667%;
  }
  .col-lg-offset-7 {
    margin-left: 58.33333333%;
  }
  .col-lg-offset-6 {
    margin-left: 50%;
  }
  .col-lg-offset-5 {
    margin-left: 41.66666667%;
  }
  .col-lg-offset-4 {
    margin-left: 33.33333333%;
  }
  .col-lg-offset-3 {
    margin-left: 25%;
  }
  .col-lg-offset-2 {
    margin-left: 16.66666667%;
  }
  .col-lg-offset-1 {
    margin-left: 8.33333333%;
  }
  .col-lg-offset-0 {
    margin-left: 0%;
  }
}
table {
  background-color: transparent;
}
caption {
  padding-top: 8px;
  padding-bottom: 8px;
  color: #777777;
  text-align: left;
}
th {
  text-align: left;
}
.table {
  width: 100%;
  max-width: 100%;
  margin-bottom: 18px;
}
.table > thead > tr > th,
.table > tbody > tr > th,
.table > tfoot > tr > th,
.table > thead > tr > td,
.table > tbody > tr > td,
.table > tfoot > tr > td {
  padding: 8px;
  line-height: 1.42857143;
  vertical-align: top;
  border-top: 1px solid #ddd;
}
.table > thead > tr > th {
  vertical-align: bottom;
  border-bottom: 2px solid #ddd;
}
.table > caption + thead > tr:first-child > th,
.table > colgroup + thead > tr:first-child > th,
.table > thead:first-child > tr:first-child > th,
.table > caption + thead > tr:first-child > td,
.table > colgroup + thead > tr:first-child > td,
.table > thead:first-child > tr:first-child > td {
  border-top: 0;
}
.table > tbody + tbody {
  border-top: 2px solid #ddd;
}
.table .table {
  background-color: #fff;
}
.table-condensed > thead > tr > th,
.table-condensed > tbody > tr > th,
.table-condensed > tfoot > tr > th,
.table-condensed > thead > tr > td,
.table-condensed > tbody > tr > td,
.table-condensed > tfoot > tr > td {
  padding: 5px;
}
.table-bordered {
  border: 1px solid #ddd;
}
.table-bordered > thead > tr > th,
.table-bordered > tbody > tr > th,
.table-bordered > tfoot > tr > th,
.table-bordered > thead > tr > td,
.table-bordered > tbody > tr > td,
.table-bordered > tfoot > tr > td {
  border: 1px solid #ddd;
}
.table-bordered > thead > tr > th,
.table-bordered > thead > tr > td {
  border-bottom-width: 2px;
}
.table-striped > tbody > tr:nth-of-type(odd) {
  background-color: #f9f9f9;
}
.table-hover > tbody > tr:hover {
  background-color: #f5f5f5;
}
table col[class*="col-"] {
  position: static;
  float: none;
  display: table-column;
}
table td[class*="col-"],
table th[class*="col-"] {
  position: static;
  float: none;
  display: table-cell;
}
.table > thead > tr > td.active,
.table > tbody > tr > td.active,
.table > tfoot > tr > td.active,
.table > thead > tr > th.active,
.table > tbody > tr > th.active,
.table > tfoot > tr > th.active,
.table > thead > tr.active > td,
.table > tbody > tr.active > td,
.table > tfoot > tr.active > td,
.table > thead > tr.active > th,
.table > tbody > tr.active > th,
.table > tfoot > tr.active > th {
  background-color: #f5f5f5;
}
.table-hover > tbody > tr > td.active:hover,
.table-hover > tbody > tr > th.active:hover,
.table-hover > tbody > tr.active:hover > td,
.table-hover > tbody > tr:hover > .active,
.table-hover > tbody > tr.active:hover > th {
  background-color: #e8e8e8;
}
.table > thead > tr > td.success,
.table > tbody > tr > td.success,
.table > tfoot > tr > td.success,
.table > thead > tr > th.success,
.table > tbody > tr > th.success,
.table > tfoot > tr > th.success,
.table > thead > tr.success > td,
.table > tbody > tr.success > td,
.table > tfoot > tr.success > td,
.table > thead > tr.success > th,
.table > tbody > tr.success > th,
.table > tfoot > tr.success > th {
  background-color: #dff0d8;
}
.table-hover > tbody > tr > td.success:hover,
.table-hover > tbody > tr > th.success:hover,
.table-hover > tbody > tr.success:hover > td,
.table-hover > tbody > tr:hover > .success,
.table-hover > tbody > tr.success:hover > th {
  background-color: #d0e9c6;
}
.table > thead > tr > td.info,
.table > tbody > tr > td.info,
.table > tfoot > tr > td.info,
.table > thead > tr > th.info,
.table > tbody > tr > th.info,
.table > tfoot > tr > th.info,
.table > thead > tr.info > td,
.table > tbody > tr.info > td,
.table > tfoot > tr.info > td,
.table > thead > tr.info > th,
.table > tbody > tr.info > th,
.table > tfoot > tr.info > th {
  background-color: #d9edf7;
}
.table-hover > tbody > tr > td.info:hover,
.table-hover > tbody > tr > th.info:hover,
.table-hover > tbody > tr.info:hover > td,
.table-hover > tbody > tr:hover > .info,
.table-hover > tbody > tr.info:hover > th {
  background-color: #c4e3f3;
}
.table > thead > tr > td.warning,
.table > tbody > tr > td.warning,
.table > tfoot > tr > td.warning,
.table > thead > tr > th.warning,
.table > tbody > tr > th.warning,
.table > tfoot > tr > th.warning,
.table > thead > tr.warning > td,
.table > tbody > tr.warning > td,
.table > tfoot > tr.warning > td,
.table > thead > tr.warning > th,
.table > tbody > tr.warning > th,
.table > tfoot > tr.warning > th {
  background-color: #fcf8e3;
}
.table-hover > tbody > tr > td.warning:hover,
.table-hover > tbody > tr > th.warning:hover,
.table-hover > tbody > tr.warning:hover > td,
.table-hover > tbody > tr:hover > .warning,
.table-hover > tbody > tr.warning:hover > th {
  background-color: #faf2cc;
}
.table > thead > tr > td.danger,
.table > tbody > tr > td.danger,
.table > tfoot > tr > td.danger,
.table > thead > tr > th.danger,
.table > tbody > tr > th.danger,
.table > tfoot > tr > th.danger,
.table > thead > tr.danger > td,
.table > tbody > tr.danger > td,
.table > tfoot > tr.danger > td,
.table > thead > tr.danger > th,
.table > tbody > tr.danger > th,
.table > tfoot > tr.danger > th {
  background-color: #f2dede;
}
.table-hover > tbody > tr > td.danger:hover,
.table-hover > tbody > tr > th.danger:hover,
.table-hover > tbody > tr.danger:hover > td,
.table-hover > tbody > tr:hover > .danger,
.table-hover > tbody > tr.danger:hover > th {
  background-color: #ebcccc;
}
.table-responsive {
  overflow-x: auto;
  min-height: 0.01%;
}
@media screen and (max-width: 767px) {
  .table-responsive {
    width: 100%;
    margin-bottom: 13.5px;
    overflow-y: hidden;
    -ms-overflow-style: -ms-autohiding-scrollbar;
    border: 1px solid #ddd;
  }
  .table-responsive > .table {
    margin-bottom: 0;
  }
  .table-responsive > .table > thead > tr > th,
  .table-responsive > .table > tbody > tr > th,
  .table-responsive > .table > tfoot > tr > th,
  .table-responsive > .table > thead > tr > td,
  .table-responsive > .table > tbody > tr > td,
  .table-responsive > .table > tfoot > tr > td {
    white-space: nowrap;
  }
  .table-responsive > .table-bordered {
    border: 0;
  }
  .table-responsive > .table-bordered > thead > tr > th:first-child,
  .table-responsive > .table-bordered > tbody > tr > th:first-child,
  .table-responsive > .table-bordered > tfoot > tr > th:first-child,
  .table-responsive > .table-bordered > thead > tr > td:first-child,
  .table-responsive > .table-bordered > tbody > tr > td:first-child,
  .table-responsive > .table-bordered > tfoot > tr > td:first-child {
    border-left: 0;
  }
  .table-responsive > .table-bordered > thead > tr > th:last-child,
  .table-responsive > .table-bordered > tbody > tr > th:last-child,
  .table-responsive > .table-bordered > tfoot > tr > th:last-child,
  .table-responsive > .table-bordered > thead > tr > td:last-child,
  .table-responsive > .table-bordered > tbody > tr > td:last-child,
  .table-responsive > .table-bordered > tfoot > tr > td:last-child {
    border-right: 0;
  }
  .table-responsive > .table-bordered > tbody > tr:last-child > th,
  .table-responsive > .table-bordered > tfoot > tr:last-child > th,
  .table-responsive > .table-bordered > tbody > tr:last-child > td,
  .table-responsive > .table-bordered > tfoot > tr:last-child > td {
    border-bottom: 0;
  }
}
fieldset {
  padding: 0;
  margin: 0;
  border: 0;
  min-width: 0;
}
legend {
  display: block;
  width: 100%;
  padding: 0;
  margin-bottom: 18px;
  font-size: 19.5px;
  line-height: inherit;
  color: #333333;
  border: 0;
  border-bottom: 1px solid #e5e5e5;
}
label {
  display: inline-block;
  max-width: 100%;
  margin-bottom: 5px;
  font-weight: bold;
}
input[type="search"] {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
input[type="radio"],
input[type="checkbox"] {
  margin: 4px 0 0;
  margin-top: 1px \9;
  line-height: normal;
}
input[type="file"] {
  display: block;
}
input[type="range"] {
  display: block;
  width: 100%;
}
select[multiple],
select[size] {
  height: auto;
}
input[type="file"]:focus,
input[type="radio"]:focus,
input[type="checkbox"]:focus {
  outline: 5px auto -webkit-focus-ring-color;
  outline-offset: -2px;
}
output {
  display: block;
  padding-top: 7px;
  font-size: 13px;
  line-height: 1.42857143;
  color: #555555;
}
.form-control {
  display: block;
  width: 100%;
  height: 32px;
  padding: 6px 12px;
  font-size: 13px;
  line-height: 1.42857143;
  color: #555555;
  background-color: #fff;
  background-image: none;
  border: 1px solid #ccc;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  -webkit-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  -o-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
}
.form-control:focus {
  border-color: #66afe9;
  outline: 0;
  -webkit-box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
  box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
}
.form-control::-moz-placeholder {
  color: #999;
  opacity: 1;
}
.form-control:-ms-input-placeholder {
  color: #999;
}
.form-control::-webkit-input-placeholder {
  color: #999;
}
.form-control::-ms-expand {
  border: 0;
  background-color: transparent;
}
.form-control[disabled],
.form-control[readonly],
fieldset[disabled] .form-control {
  background-color: #eeeeee;
  opacity: 1;
}
.form-control[disabled],
fieldset[disabled] .form-control {
  cursor: not-allowed;
}
textarea.form-control {
  height: auto;
}
input[type="search"] {
  -webkit-appearance: none;
}
@media screen and (-webkit-min-device-pixel-ratio: 0) {
  input[type="date"].form-control,
  input[type="time"].form-control,
  input[type="datetime-local"].form-control,
  input[type="month"].form-control {
    line-height: 32px;
  }
  input[type="date"].input-sm,
  input[type="time"].input-sm,
  input[type="datetime-local"].input-sm,
  input[type="month"].input-sm,
  .input-group-sm input[type="date"],
  .input-group-sm input[type="time"],
  .input-group-sm input[type="datetime-local"],
  .input-group-sm input[type="month"] {
    line-height: 30px;
  }
  input[type="date"].input-lg,
  input[type="time"].input-lg,
  input[type="datetime-local"].input-lg,
  input[type="month"].input-lg,
  .input-group-lg input[type="date"],
  .input-group-lg input[type="time"],
  .input-group-lg input[type="datetime-local"],
  .input-group-lg input[type="month"] {
    line-height: 45px;
  }
}
.form-group {
  margin-bottom: 15px;
}
.radio,
.checkbox {
  position: relative;
  display: block;
  margin-top: 10px;
  margin-bottom: 10px;
}
.radio label,
.checkbox label {
  min-height: 18px;
  padding-left: 20px;
  margin-bottom: 0;
  font-weight: normal;
  cursor: pointer;
}
.radio input[type="radio"],
.radio-inline input[type="radio"],
.checkbox input[type="checkbox"],
.checkbox-inline input[type="checkbox"] {
  position: absolute;
  margin-left: -20px;
  margin-top: 4px \9;
}
.radio + .radio,
.checkbox + .checkbox {
  margin-top: -5px;
}
.radio-inline,
.checkbox-inline {
  position: relative;
  display: inline-block;
  padding-left: 20px;
  margin-bottom: 0;
  vertical-align: middle;
  font-weight: normal;
  cursor: pointer;
}
.radio-inline + .radio-inline,
.checkbox-inline + .checkbox-inline {
  margin-top: 0;
  margin-left: 10px;
}
input[type="radio"][disabled],
input[type="checkbox"][disabled],
input[type="radio"].disabled,
input[type="checkbox"].disabled,
fieldset[disabled] input[type="radio"],
fieldset[disabled] input[type="checkbox"] {
  cursor: not-allowed;
}
.radio-inline.disabled,
.checkbox-inline.disabled,
fieldset[disabled] .radio-inline,
fieldset[disabled] .checkbox-inline {
  cursor: not-allowed;
}
.radio.disabled label,
.checkbox.disabled label,
fieldset[disabled] .radio label,
fieldset[disabled] .checkbox label {
  cursor: not-allowed;
}
.form-control-static {
  padding-top: 7px;
  padding-bottom: 7px;
  margin-bottom: 0;
  min-height: 31px;
}
.form-control-static.input-lg,
.form-control-static.input-sm {
  padding-left: 0;
  padding-right: 0;
}
.input-sm {
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
select.input-sm {
  height: 30px;
  line-height: 30px;
}
textarea.input-sm,
select[multiple].input-sm {
  height: auto;
}
.form-group-sm .form-control {
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
.form-group-sm select.form-control {
  height: 30px;
  line-height: 30px;
}
.form-group-sm textarea.form-control,
.form-group-sm select[multiple].form-control {
  height: auto;
}
.form-group-sm .form-control-static {
  height: 30px;
  min-height: 30px;
  padding: 6px 10px;
  font-size: 12px;
  line-height: 1.5;
}
.input-lg {
  height: 45px;
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
  border-radius: 3px;
}
select.input-lg {
  height: 45px;
  line-height: 45px;
}
textarea.input-lg,
select[multiple].input-lg {
  height: auto;
}
.form-group-lg .form-control {
  height: 45px;
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
  border-radius: 3px;
}
.form-group-lg select.form-control {
  height: 45px;
  line-height: 45px;
}
.form-group-lg textarea.form-control,
.form-group-lg select[multiple].form-control {
  height: auto;
}
.form-group-lg .form-control-static {
  height: 45px;
  min-height: 35px;
  padding: 11px 16px;
  font-size: 17px;
  line-height: 1.3333333;
}
.has-feedback {
  position: relative;
}
.has-feedback .form-control {
  padding-right: 40px;
}
.form-control-feedback {
  position: absolute;
  top: 0;
  right: 0;
  z-index: 2;
  display: block;
  width: 32px;
  height: 32px;
  line-height: 32px;
  text-align: center;
  pointer-events: none;
}
.input-lg + .form-control-feedback,
.input-group-lg + .form-control-feedback,
.form-group-lg .form-control + .form-control-feedback {
  width: 45px;
  height: 45px;
  line-height: 45px;
}
.input-sm + .form-control-feedback,
.input-group-sm + .form-control-feedback,
.form-group-sm .form-control + .form-control-feedback {
  width: 30px;
  height: 30px;
  line-height: 30px;
}
.has-success .help-block,
.has-success .control-label,
.has-success .radio,
.has-success .checkbox,
.has-success .radio-inline,
.has-success .checkbox-inline,
.has-success.radio label,
.has-success.checkbox label,
.has-success.radio-inline label,
.has-success.checkbox-inline label {
  color: #3c763d;
}
.has-success .form-control {
  border-color: #3c763d;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
}
.has-success .form-control:focus {
  border-color: #2b542c;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #67b168;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #67b168;
}
.has-success .input-group-addon {
  color: #3c763d;
  border-color: #3c763d;
  background-color: #dff0d8;
}
.has-success .form-control-feedback {
  color: #3c763d;
}
.has-warning .help-block,
.has-warning .control-label,
.has-warning .radio,
.has-warning .checkbox,
.has-warning .radio-inline,
.has-warning .checkbox-inline,
.has-warning.radio label,
.has-warning.checkbox label,
.has-warning.radio-inline label,
.has-warning.checkbox-inline label {
  color: #8a6d3b;
}
.has-warning .form-control {
  border-color: #8a6d3b;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
}
.has-warning .form-control:focus {
  border-color: #66512c;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #c0a16b;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #c0a16b;
}
.has-warning .input-group-addon {
  color: #8a6d3b;
  border-color: #8a6d3b;
  background-color: #fcf8e3;
}
.has-warning .form-control-feedback {
  color: #8a6d3b;
}
.has-error .help-block,
.has-error .control-label,
.has-error .radio,
.has-error .checkbox,
.has-error .radio-inline,
.has-error .checkbox-inline,
.has-error.radio label,
.has-error.checkbox label,
.has-error.radio-inline label,
.has-error.checkbox-inline label {
  color: #a94442;
}
.has-error .form-control {
  border-color: #a94442;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
}
.has-error .form-control:focus {
  border-color: #843534;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #ce8483;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #ce8483;
}
.has-error .input-group-addon {
  color: #a94442;
  border-color: #a94442;
  background-color: #f2dede;
}
.has-error .form-control-feedback {
  color: #a94442;
}
.has-feedback label ~ .form-control-feedback {
  top: 23px;
}
.has-feedback label.sr-only ~ .form-control-feedback {
  top: 0;
}
.help-block {
  display: block;
  margin-top: 5px;
  margin-bottom: 10px;
  color: #404040;
}
@media (min-width: 768px) {
  .form-inline .form-group {
    display: inline-block;
    margin-bottom: 0;
    vertical-align: middle;
  }
  .form-inline .form-control {
    display: inline-block;
    width: auto;
    vertical-align: middle;
  }
  .form-inline .form-control-static {
    display: inline-block;
  }
  .form-inline .input-group {
    display: inline-table;
    vertical-align: middle;
  }
  .form-inline .input-group .input-group-addon,
  .form-inline .input-group .input-group-btn,
  .form-inline .input-group .form-control {
    width: auto;
  }
  .form-inline .input-group > .form-control {
    width: 100%;
  }
  .form-inline .control-label {
    margin-bottom: 0;
    vertical-align: middle;
  }
  .form-inline .radio,
  .form-inline .checkbox {
    display: inline-block;
    margin-top: 0;
    margin-bottom: 0;
    vertical-align: middle;
  }
  .form-inline .radio label,
  .form-inline .checkbox label {
    padding-left: 0;
  }
  .form-inline .radio input[type="radio"],
  .form-inline .checkbox input[type="checkbox"] {
    position: relative;
    margin-left: 0;
  }
  .form-inline .has-feedback .form-control-feedback {
    top: 0;
  }
}
.form-horizontal .radio,
.form-horizontal .checkbox,
.form-horizontal .radio-inline,
.form-horizontal .checkbox-inline {
  margin-top: 0;
  margin-bottom: 0;
  padding-top: 7px;
}
.form-horizontal .radio,
.form-horizontal .checkbox {
  min-height: 25px;
}
.form-horizontal .form-group {
  margin-left: 0px;
  margin-right: 0px;
}
@media (min-width: 768px) {
  .form-horizontal .control-label {
    text-align: right;
    margin-bottom: 0;
    padding-top: 7px;
  }
}
.form-horizontal .has-feedback .form-control-feedback {
  right: 0px;
}
@media (min-width: 768px) {
  .form-horizontal .form-group-lg .control-label {
    padding-top: 11px;
    font-size: 17px;
  }
}
@media (min-width: 768px) {
  .form-horizontal .form-group-sm .control-label {
    padding-top: 6px;
    font-size: 12px;
  }
}
.btn {
  display: inline-block;
  margin-bottom: 0;
  font-weight: normal;
  text-align: center;
  vertical-align: middle;
  touch-action: manipulation;
  cursor: pointer;
  background-image: none;
  border: 1px solid transparent;
  white-space: nowrap;
  padding: 6px 12px;
  font-size: 13px;
  line-height: 1.42857143;
  border-radius: 2px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
.btn:focus,
.btn:active:focus,
.btn.active:focus,
.btn.focus,
.btn:active.focus,
.btn.active.focus {
  outline: 5px auto -webkit-focus-ring-color;
  outline-offset: -2px;
}
.btn:hover,
.btn:focus,
.btn.focus {
  color: #333;
  text-decoration: none;
}
.btn:active,
.btn.active {
  outline: 0;
  background-image: none;
  -webkit-box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
  box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
}
.btn.disabled,
.btn[disabled],
fieldset[disabled] .btn {
  cursor: not-allowed;
  opacity: 0.65;
  filter: alpha(opacity=65);
  -webkit-box-shadow: none;
  box-shadow: none;
}
a.btn.disabled,
fieldset[disabled] a.btn {
  pointer-events: none;
}
.btn-default {
  color: #333;
  background-color: #fff;
  border-color: #ccc;
}
.btn-default:focus,
.btn-default.focus {
  color: #333;
  background-color: #e6e6e6;
  border-color: #8c8c8c;
}
.btn-default:hover {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
.btn-default:active,
.btn-default.active,
.open > .dropdown-toggle.btn-default {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
.btn-default:active:hover,
.btn-default.active:hover,
.open > .dropdown-toggle.btn-default:hover,
.btn-default:active:focus,
.btn-default.active:focus,
.open > .dropdown-toggle.btn-default:focus,
.btn-default:active.focus,
.btn-default.active.focus,
.open > .dropdown-toggle.btn-default.focus {
  color: #333;
  background-color: #d4d4d4;
  border-color: #8c8c8c;
}
.btn-default:active,
.btn-default.active,
.open > .dropdown-toggle.btn-default {
  background-image: none;
}
.btn-default.disabled:hover,
.btn-default[disabled]:hover,
fieldset[disabled] .btn-default:hover,
.btn-default.disabled:focus,
.btn-default[disabled]:focus,
fieldset[disabled] .btn-default:focus,
.btn-default.disabled.focus,
.btn-default[disabled].focus,
fieldset[disabled] .btn-default.focus {
  background-color: #fff;
  border-color: #ccc;
}
.btn-default .badge {
  color: #fff;
  background-color: #333;
}
.btn-primary {
  color: #fff;
  background-color: #337ab7;
  border-color: #2e6da4;
}
.btn-primary:focus,
.btn-primary.focus {
  color: #fff;
  background-color: #286090;
  border-color: #122b40;
}
.btn-primary:hover {
  color: #fff;
  background-color: #286090;
  border-color: #204d74;
}
.btn-primary:active,
.btn-primary.active,
.open > .dropdown-toggle.btn-primary {
  color: #fff;
  background-color: #286090;
  border-color: #204d74;
}
.btn-primary:active:hover,
.btn-primary.active:hover,
.open > .dropdown-toggle.btn-primary:hover,
.btn-primary:active:focus,
.btn-primary.active:focus,
.open > .dropdown-toggle.btn-primary:focus,
.btn-primary:active.focus,
.btn-primary.active.focus,
.open > .dropdown-toggle.btn-primary.focus {
  color: #fff;
  background-color: #204d74;
  border-color: #122b40;
}
.btn-primary:active,
.btn-primary.active,
.open > .dropdown-toggle.btn-primary {
  background-image: none;
}
.btn-primary.disabled:hover,
.btn-primary[disabled]:hover,
fieldset[disabled] .btn-primary:hover,
.btn-primary.disabled:focus,
.btn-primary[disabled]:focus,
fieldset[disabled] .btn-primary:focus,
.btn-primary.disabled.focus,
.btn-primary[disabled].focus,
fieldset[disabled] .btn-primary.focus {
  background-color: #337ab7;
  border-color: #2e6da4;
}
.btn-primary .badge {
  color: #337ab7;
  background-color: #fff;
}
.btn-success {
  color: #fff;
  background-color: #5cb85c;
  border-color: #4cae4c;
}
.btn-success:focus,
.btn-success.focus {
  color: #fff;
  background-color: #449d44;
  border-color: #255625;
}
.btn-success:hover {
  color: #fff;
  background-color: #449d44;
  border-color: #398439;
}
.btn-success:active,
.btn-success.active,
.open > .dropdown-toggle.btn-success {
  color: #fff;
  background-color: #449d44;
  border-color: #398439;
}
.btn-success:active:hover,
.btn-success.active:hover,
.open > .dropdown-toggle.btn-success:hover,
.btn-success:active:focus,
.btn-success.active:focus,
.open > .dropdown-toggle.btn-success:focus,
.btn-success:active.focus,
.btn-success.active.focus,
.open > .dropdown-toggle.btn-success.focus {
  color: #fff;
  background-color: #398439;
  border-color: #255625;
}
.btn-success:active,
.btn-success.active,
.open > .dropdown-toggle.btn-success {
  background-image: none;
}
.btn-success.disabled:hover,
.btn-success[disabled]:hover,
fieldset[disabled] .btn-success:hover,
.btn-success.disabled:focus,
.btn-success[disabled]:focus,
fieldset[disabled] .btn-success:focus,
.btn-success.disabled.focus,
.btn-success[disabled].focus,
fieldset[disabled] .btn-success.focus {
  background-color: #5cb85c;
  border-color: #4cae4c;
}
.btn-success .badge {
  color: #5cb85c;
  background-color: #fff;
}
.btn-info {
  color: #fff;
  background-color: #5bc0de;
  border-color: #46b8da;
}
.btn-info:focus,
.btn-info.focus {
  color: #fff;
  background-color: #31b0d5;
  border-color: #1b6d85;
}
.btn-info:hover {
  color: #fff;
  background-color: #31b0d5;
  border-color: #269abc;
}
.btn-info:active,
.btn-info.active,
.open > .dropdown-toggle.btn-info {
  color: #fff;
  background-color: #31b0d5;
  border-color: #269abc;
}
.btn-info:active:hover,
.btn-info.active:hover,
.open > .dropdown-toggle.btn-info:hover,
.btn-info:active:focus,
.btn-info.active:focus,
.open > .dropdown-toggle.btn-info:focus,
.btn-info:active.focus,
.btn-info.active.focus,
.open > .dropdown-toggle.btn-info.focus {
  color: #fff;
  background-color: #269abc;
  border-color: #1b6d85;
}
.btn-info:active,
.btn-info.active,
.open > .dropdown-toggle.btn-info {
  background-image: none;
}
.btn-info.disabled:hover,
.btn-info[disabled]:hover,
fieldset[disabled] .btn-info:hover,
.btn-info.disabled:focus,
.btn-info[disabled]:focus,
fieldset[disabled] .btn-info:focus,
.btn-info.disabled.focus,
.btn-info[disabled].focus,
fieldset[disabled] .btn-info.focus {
  background-color: #5bc0de;
  border-color: #46b8da;
}
.btn-info .badge {
  color: #5bc0de;
  background-color: #fff;
}
.btn-warning {
  color: #fff;
  background-color: #f0ad4e;
  border-color: #eea236;
}
.btn-warning:focus,
.btn-warning.focus {
  color: #fff;
  background-color: #ec971f;
  border-color: #985f0d;
}
.btn-warning:hover {
  color: #fff;
  background-color: #ec971f;
  border-color: #d58512;
}
.btn-warning:active,
.btn-warning.active,
.open > .dropdown-toggle.btn-warning {
  color: #fff;
  background-color: #ec971f;
  border-color: #d58512;
}
.btn-warning:active:hover,
.btn-warning.active:hover,
.open > .dropdown-toggle.btn-warning:hover,
.btn-warning:active:focus,
.btn-warning.active:focus,
.open > .dropdown-toggle.btn-warning:focus,
.btn-warning:active.focus,
.btn-warning.active.focus,
.open > .dropdown-toggle.btn-warning.focus {
  color: #fff;
  background-color: #d58512;
  border-color: #985f0d;
}
.btn-warning:active,
.btn-warning.active,
.open > .dropdown-toggle.btn-warning {
  background-image: none;
}
.btn-warning.disabled:hover,
.btn-warning[disabled]:hover,
fieldset[disabled] .btn-warning:hover,
.btn-warning.disabled:focus,
.btn-warning[disabled]:focus,
fieldset[disabled] .btn-warning:focus,
.btn-warning.disabled.focus,
.btn-warning[disabled].focus,
fieldset[disabled] .btn-warning.focus {
  background-color: #f0ad4e;
  border-color: #eea236;
}
.btn-warning .badge {
  color: #f0ad4e;
  background-color: #fff;
}
.btn-danger {
  color: #fff;
  background-color: #d9534f;
  border-color: #d43f3a;
}
.btn-danger:focus,
.btn-danger.focus {
  color: #fff;
  background-color: #c9302c;
  border-color: #761c19;
}
.btn-danger:hover {
  color: #fff;
  background-color: #c9302c;
  border-color: #ac2925;
}
.btn-danger:active,
.btn-danger.active,
.open > .dropdown-toggle.btn-danger {
  color: #fff;
  background-color: #c9302c;
  border-color: #ac2925;
}
.btn-danger:active:hover,
.btn-danger.active:hover,
.open > .dropdown-toggle.btn-danger:hover,
.btn-danger:active:focus,
.btn-danger.active:focus,
.open > .dropdown-toggle.btn-danger:focus,
.btn-danger:active.focus,
.btn-danger.active.focus,
.open > .dropdown-toggle.btn-danger.focus {
  color: #fff;
  background-color: #ac2925;
  border-color: #761c19;
}
.btn-danger:active,
.btn-danger.active,
.open > .dropdown-toggle.btn-danger {
  background-image: none;
}
.btn-danger.disabled:hover,
.btn-danger[disabled]:hover,
fieldset[disabled] .btn-danger:hover,
.btn-danger.disabled:focus,
.btn-danger[disabled]:focus,
fieldset[disabled] .btn-danger:focus,
.btn-danger.disabled.focus,
.btn-danger[disabled].focus,
fieldset[disabled] .btn-danger.focus {
  background-color: #d9534f;
  border-color: #d43f3a;
}
.btn-danger .badge {
  color: #d9534f;
  background-color: #fff;
}
.btn-link {
  color: #337ab7;
  font-weight: normal;
  border-radius: 0;
}
.btn-link,
.btn-link:active,
.btn-link.active,
.btn-link[disabled],
fieldset[disabled] .btn-link {
  background-color: transparent;
  -webkit-box-shadow: none;
  box-shadow: none;
}
.btn-link,
.btn-link:hover,
.btn-link:focus,
.btn-link:active {
  border-color: transparent;
}
.btn-link:hover,
.btn-link:focus {
  color: #23527c;
  text-decoration: underline;
  background-color: transparent;
}
.btn-link[disabled]:hover,
fieldset[disabled] .btn-link:hover,
.btn-link[disabled]:focus,
fieldset[disabled] .btn-link:focus {
  color: #777777;
  text-decoration: none;
}
.btn-lg,
.btn-group-lg > .btn {
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
  border-radius: 3px;
}
.btn-sm,
.btn-group-sm > .btn {
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
.btn-xs,
.btn-group-xs > .btn {
  padding: 1px 5px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
.btn-block {
  display: block;
  width: 100%;
}
.btn-block + .btn-block {
  margin-top: 5px;
}
input[type="submit"].btn-block,
input[type="reset"].btn-block,
input[type="button"].btn-block {
  width: 100%;
}
.fade {
  opacity: 0;
  -webkit-transition: opacity 0.15s linear;
  -o-transition: opacity 0.15s linear;
  transition: opacity 0.15s linear;
}
.fade.in {
  opacity: 1;
}
.collapse {
  display: none;
}
.collapse.in {
  display: block;
}
tr.collapse.in {
  display: table-row;
}
tbody.collapse.in {
  display: table-row-group;
}
.collapsing {
  position: relative;
  height: 0;
  overflow: hidden;
  -webkit-transition-property: height, visibility;
  transition-property: height, visibility;
  -webkit-transition-duration: 0.35s;
  transition-duration: 0.35s;
  -webkit-transition-timing-function: ease;
  transition-timing-function: ease;
}
.caret {
  display: inline-block;
  width: 0;
  height: 0;
  margin-left: 2px;
  vertical-align: middle;
  border-top: 4px dashed;
  border-top: 4px solid \9;
  border-right: 4px solid transparent;
  border-left: 4px solid transparent;
}
.dropup,
.dropdown {
  position: relative;
}
.dropdown-toggle:focus {
  outline: 0;
}
.dropdown-menu {
  position: absolute;
  top: 100%;
  left: 0;
  z-index: 1000;
  display: none;
  float: left;
  min-width: 160px;
  padding: 5px 0;
  margin: 2px 0 0;
  list-style: none;
  font-size: 13px;
  text-align: left;
  background-color: #fff;
  border: 1px solid #ccc;
  border: 1px solid rgba(0, 0, 0, 0.15);
  border-radius: 2px;
  -webkit-box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175);
  background-clip: padding-box;
}
.dropdown-menu.pull-right {
  right: 0;
  left: auto;
}
.dropdown-menu .divider {
  height: 1px;
  margin: 8px 0;
  overflow: hidden;
  background-color: #e5e5e5;
}
.dropdown-menu > li > a {
  display: block;
  padding: 3px 20px;
  clear: both;
  font-weight: normal;
  line-height: 1.42857143;
  color: #333333;
  white-space: nowrap;
}
.dropdown-menu > li > a:hover,
.dropdown-menu > li > a:focus {
  text-decoration: none;
  color: #262626;
  background-color: #f5f5f5;
}
.dropdown-menu > .active > a,
.dropdown-menu > .active > a:hover,
.dropdown-menu > .active > a:focus {
  color: #fff;
  text-decoration: none;
  outline: 0;
  background-color: #337ab7;
}
.dropdown-menu > .disabled > a,
.dropdown-menu > .disabled > a:hover,
.dropdown-menu > .disabled > a:focus {
  color: #777777;
}
.dropdown-menu > .disabled > a:hover,
.dropdown-menu > .disabled > a:focus {
  text-decoration: none;
  background-color: transparent;
  background-image: none;
  filter: progid:DXImageTransform.Microsoft.gradient(enabled = false);
  cursor: not-allowed;
}
.open > .dropdown-menu {
  display: block;
}
.open > a {
  outline: 0;
}
.dropdown-menu-right {
  left: auto;
  right: 0;
}
.dropdown-menu-left {
  left: 0;
  right: auto;
}
.dropdown-header {
  display: block;
  padding: 3px 20px;
  font-size: 12px;
  line-height: 1.42857143;
  color: #777777;
  white-space: nowrap;
}
.dropdown-backdrop {
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  top: 0;
  z-index: 990;
}
.pull-right > .dropdown-menu {
  right: 0;
  left: auto;
}
.dropup .caret,
.navbar-fixed-bottom .dropdown .caret {
  border-top: 0;
  border-bottom: 4px dashed;
  border-bottom: 4px solid \9;
  content: "";
}
.dropup .dropdown-menu,
.navbar-fixed-bottom .dropdown .dropdown-menu {
  top: auto;
  bottom: 100%;
  margin-bottom: 2px;
}
@media (min-width: 541px) {
  .navbar-right .dropdown-menu {
    left: auto;
    right: 0;
  }
  .navbar-right .dropdown-menu-left {
    left: 0;
    right: auto;
  }
}
.btn-group,
.btn-group-vertical {
  position: relative;
  display: inline-block;
  vertical-align: middle;
}
.btn-group > .btn,
.btn-group-vertical > .btn {
  position: relative;
  float: left;
}
.btn-group > .btn:hover,
.btn-group-vertical > .btn:hover,
.btn-group > .btn:focus,
.btn-group-vertical > .btn:focus,
.btn-group > .btn:active,
.btn-group-vertical > .btn:active,
.btn-group > .btn.active,
.btn-group-vertical > .btn.active {
  z-index: 2;
}
.btn-group .btn + .btn,
.btn-group .btn + .btn-group,
.btn-group .btn-group + .btn,
.btn-group .btn-group + .btn-group {
  margin-left: -1px;
}
.btn-toolbar {
  margin-left: -5px;
}
.btn-toolbar .btn,
.btn-toolbar .btn-group,
.btn-toolbar .input-group {
  float: left;
}
.btn-toolbar > .btn,
.btn-toolbar > .btn-group,
.btn-toolbar > .input-group {
  margin-left: 5px;
}
.btn-group > .btn:not(:first-child):not(:last-child):not(.dropdown-toggle) {
  border-radius: 0;
}
.btn-group > .btn:first-child {
  margin-left: 0;
}
.btn-group > .btn:first-child:not(:last-child):not(.dropdown-toggle) {
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
}
.btn-group > .btn:last-child:not(:first-child),
.btn-group > .dropdown-toggle:not(:first-child) {
  border-bottom-left-radius: 0;
  border-top-left-radius: 0;
}
.btn-group > .btn-group {
  float: left;
}
.btn-group > .btn-group:not(:first-child):not(:last-child) > .btn {
  border-radius: 0;
}
.btn-group > .btn-group:first-child:not(:last-child) > .btn:last-child,
.btn-group > .btn-group:first-child:not(:last-child) > .dropdown-toggle {
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
}
.btn-group > .btn-group:last-child:not(:first-child) > .btn:first-child {
  border-bottom-left-radius: 0;
  border-top-left-radius: 0;
}
.btn-group .dropdown-toggle:active,
.btn-group.open .dropdown-toggle {
  outline: 0;
}
.btn-group > .btn + .dropdown-toggle {
  padding-left: 8px;
  padding-right: 8px;
}
.btn-group > .btn-lg + .dropdown-toggle {
  padding-left: 12px;
  padding-right: 12px;
}
.btn-group.open .dropdown-toggle {
  -webkit-box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
  box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
}
.btn-group.open .dropdown-toggle.btn-link {
  -webkit-box-shadow: none;
  box-shadow: none;
}
.btn .caret {
  margin-left: 0;
}
.btn-lg .caret {
  border-width: 5px 5px 0;
  border-bottom-width: 0;
}
.dropup .btn-lg .caret {
  border-width: 0 5px 5px;
}
.btn-group-vertical > .btn,
.btn-group-vertical > .btn-group,
.btn-group-vertical > .btn-group > .btn {
  display: block;
  float: none;
  width: 100%;
  max-width: 100%;
}
.btn-group-vertical > .btn-group > .btn {
  float: none;
}
.btn-group-vertical > .btn + .btn,
.btn-group-vertical > .btn + .btn-group,
.btn-group-vertical > .btn-group + .btn,
.btn-group-vertical > .btn-group + .btn-group {
  margin-top: -1px;
  margin-left: 0;
}
.btn-group-vertical > .btn:not(:first-child):not(:last-child) {
  border-radius: 0;
}
.btn-group-vertical > .btn:first-child:not(:last-child) {
  border-top-right-radius: 2px;
  border-top-left-radius: 2px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.btn-group-vertical > .btn:last-child:not(:first-child) {
  border-top-right-radius: 0;
  border-top-left-radius: 0;
  border-bottom-right-radius: 2px;
  border-bottom-left-radius: 2px;
}
.btn-group-vertical > .btn-group:not(:first-child):not(:last-child) > .btn {
  border-radius: 0;
}
.btn-group-vertical > .btn-group:first-child:not(:last-child) > .btn:last-child,
.btn-group-vertical > .btn-group:first-child:not(:last-child) > .dropdown-toggle {
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.btn-group-vertical > .btn-group:last-child:not(:first-child) > .btn:first-child {
  border-top-right-radius: 0;
  border-top-left-radius: 0;
}
.btn-group-justified {
  display: table;
  width: 100%;
  table-layout: fixed;
  border-collapse: separate;
}
.btn-group-justified > .btn,
.btn-group-justified > .btn-group {
  float: none;
  display: table-cell;
  width: 1%;
}
.btn-group-justified > .btn-group .btn {
  width: 100%;
}
.btn-group-justified > .btn-group .dropdown-menu {
  left: auto;
}
[data-toggle="buttons"] > .btn input[type="radio"],
[data-toggle="buttons"] > .btn-group > .btn input[type="radio"],
[data-toggle="buttons"] > .btn input[type="checkbox"],
[data-toggle="buttons"] > .btn-group > .btn input[type="checkbox"] {
  position: absolute;
  clip: rect(0, 0, 0, 0);
  pointer-events: none;
}
.input-group {
  position: relative;
  display: table;
  border-collapse: separate;
}
.input-group[class*="col-"] {
  float: none;
  padding-left: 0;
  padding-right: 0;
}
.input-group .form-control {
  position: relative;
  z-index: 2;
  float: left;
  width: 100%;
  margin-bottom: 0;
}
.input-group .form-control:focus {
  z-index: 3;
}
.input-group-lg > .form-control,
.input-group-lg > .input-group-addon,
.input-group-lg > .input-group-btn > .btn {
  height: 45px;
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
  border-radius: 3px;
}
select.input-group-lg > .form-control,
select.input-group-lg > .input-group-addon,
select.input-group-lg > .input-group-btn > .btn {
  height: 45px;
  line-height: 45px;
}
textarea.input-group-lg > .form-control,
textarea.input-group-lg > .input-group-addon,
textarea.input-group-lg > .input-group-btn > .btn,
select[multiple].input-group-lg > .form-control,
select[multiple].input-group-lg > .input-group-addon,
select[multiple].input-group-lg > .input-group-btn > .btn {
  height: auto;
}
.input-group-sm > .form-control,
.input-group-sm > .input-group-addon,
.input-group-sm > .input-group-btn > .btn {
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
select.input-group-sm > .form-control,
select.input-group-sm > .input-group-addon,
select.input-group-sm > .input-group-btn > .btn {
  height: 30px;
  line-height: 30px;
}
textarea.input-group-sm > .form-control,
textarea.input-group-sm > .input-group-addon,
textarea.input-group-sm > .input-group-btn > .btn,
select[multiple].input-group-sm > .form-control,
select[multiple].input-group-sm > .input-group-addon,
select[multiple].input-group-sm > .input-group-btn > .btn {
  height: auto;
}
.input-group-addon,
.input-group-btn,
.input-group .form-control {
  display: table-cell;
}
.input-group-addon:not(:first-child):not(:last-child),
.input-group-btn:not(:first-child):not(:last-child),
.input-group .form-control:not(:first-child):not(:last-child) {
  border-radius: 0;
}
.input-group-addon,
.input-group-btn {
  width: 1%;
  white-space: nowrap;
  vertical-align: middle;
}
.input-group-addon {
  padding: 6px 12px;
  font-size: 13px;
  font-weight: normal;
  line-height: 1;
  color: #555555;
  text-align: center;
  background-color: #eeeeee;
  border: 1px solid #ccc;
  border-radius: 2px;
}
.input-group-addon.input-sm {
  padding: 5px 10px;
  font-size: 12px;
  border-radius: 1px;
}
.input-group-addon.input-lg {
  padding: 10px 16px;
  font-size: 17px;
  border-radius: 3px;
}
.input-group-addon input[type="radio"],
.input-group-addon input[type="checkbox"] {
  margin-top: 0;
}
.input-group .form-control:first-child,
.input-group-addon:first-child,
.input-group-btn:first-child > .btn,
.input-group-btn:first-child > .btn-group > .btn,
.input-group-btn:first-child > .dropdown-toggle,
.input-group-btn:last-child > .btn:not(:last-child):not(.dropdown-toggle),
.input-group-btn:last-child > .btn-group:not(:last-child) > .btn {
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
}
.input-group-addon:first-child {
  border-right: 0;
}
.input-group .form-control:last-child,
.input-group-addon:last-child,
.input-group-btn:last-child > .btn,
.input-group-btn:last-child > .btn-group > .btn,
.input-group-btn:last-child > .dropdown-toggle,
.input-group-btn:first-child > .btn:not(:first-child),
.input-group-btn:first-child > .btn-group:not(:first-child) > .btn {
  border-bottom-left-radius: 0;
  border-top-left-radius: 0;
}
.input-group-addon:last-child {
  border-left: 0;
}
.input-group-btn {
  position: relative;
  font-size: 0;
  white-space: nowrap;
}
.input-group-btn > .btn {
  position: relative;
}
.input-group-btn > .btn + .btn {
  margin-left: -1px;
}
.input-group-btn > .btn:hover,
.input-group-btn > .btn:focus,
.input-group-btn > .btn:active {
  z-index: 2;
}
.input-group-btn:first-child > .btn,
.input-group-btn:first-child > .btn-group {
  margin-right: -1px;
}
.input-group-btn:last-child > .btn,
.input-group-btn:last-child > .btn-group {
  z-index: 2;
  margin-left: -1px;
}
.nav {
  margin-bottom: 0;
  padding-left: 0;
  list-style: none;
}
.nav > li {
  position: relative;
  display: block;
}
.nav > li > a {
  position: relative;
  display: block;
  padding: 10px 15px;
}
.nav > li > a:hover,
.nav > li > a:focus {
  text-decoration: none;
  background-color: #eeeeee;
}
.nav > li.disabled > a {
  color: #777777;
}
.nav > li.disabled > a:hover,
.nav > li.disabled > a:focus {
  color: #777777;
  text-decoration: none;
  background-color: transparent;
  cursor: not-allowed;
}
.nav .open > a,
.nav .open > a:hover,
.nav .open > a:focus {
  background-color: #eeeeee;
  border-color: #337ab7;
}
.nav .nav-divider {
  height: 1px;
  margin: 8px 0;
  overflow: hidden;
  background-color: #e5e5e5;
}
.nav > li > a > img {
  max-width: none;
}
.nav-tabs {
  border-bottom: 1px solid #ddd;
}
.nav-tabs > li {
  float: left;
  margin-bottom: -1px;
}
.nav-tabs > li > a {
  margin-right: 2px;
  line-height: 1.42857143;
  border: 1px solid transparent;
  border-radius: 2px 2px 0 0;
}
.nav-tabs > li > a:hover {
  border-color: #eeeeee #eeeeee #ddd;
}
.nav-tabs > li.active > a,
.nav-tabs > li.active > a:hover,
.nav-tabs > li.active > a:focus {
  color: #555555;
  background-color: #fff;
  border: 1px solid #ddd;
  border-bottom-color: transparent;
  cursor: default;
}
.nav-tabs.nav-justified {
  width: 100%;
  border-bottom: 0;
}
.nav-tabs.nav-justified > li {
  float: none;
}
.nav-tabs.nav-justified > li > a {
  text-align: center;
  margin-bottom: 5px;
}
.nav-tabs.nav-justified > .dropdown .dropdown-menu {
  top: auto;
  left: auto;
}
@media (min-width: 768px) {
  .nav-tabs.nav-justified > li {
    display: table-cell;
    width: 1%;
  }
  .nav-tabs.nav-justified > li > a {
    margin-bottom: 0;
  }
}
.nav-tabs.nav-justified > li > a {
  margin-right: 0;
  border-radius: 2px;
}
.nav-tabs.nav-justified > .active > a,
.nav-tabs.nav-justified > .active > a:hover,
.nav-tabs.nav-justified > .active > a:focus {
  border: 1px solid #ddd;
}
@media (min-width: 768px) {
  .nav-tabs.nav-justified > li > a {
    border-bottom: 1px solid #ddd;
    border-radius: 2px 2px 0 0;
  }
  .nav-tabs.nav-justified > .active > a,
  .nav-tabs.nav-justified > .active > a:hover,
  .nav-tabs.nav-justified > .active > a:focus {
    border-bottom-color: #fff;
  }
}
.nav-pills > li {
  float: left;
}
.nav-pills > li > a {
  border-radius: 2px;
}
.nav-pills > li + li {
  margin-left: 2px;
}
.nav-pills > li.active > a,
.nav-pills > li.active > a:hover,
.nav-pills > li.active > a:focus {
  color: #fff;
  background-color: #337ab7;
}
.nav-stacked > li {
  float: none;
}
.nav-stacked > li + li {
  margin-top: 2px;
  margin-left: 0;
}
.nav-justified {
  width: 100%;
}
.nav-justified > li {
  float: none;
}
.nav-justified > li > a {
  text-align: center;
  margin-bottom: 5px;
}
.nav-justified > .dropdown .dropdown-menu {
  top: auto;
  left: auto;
}
@media (min-width: 768px) {
  .nav-justified > li {
    display: table-cell;
    width: 1%;
  }
  .nav-justified > li > a {
    margin-bottom: 0;
  }
}
.nav-tabs-justified {
  border-bottom: 0;
}
.nav-tabs-justified > li > a {
  margin-right: 0;
  border-radius: 2px;
}
.nav-tabs-justified > .active > a,
.nav-tabs-justified > .active > a:hover,
.nav-tabs-justified > .active > a:focus {
  border: 1px solid #ddd;
}
@media (min-width: 768px) {
  .nav-tabs-justified > li > a {
    border-bottom: 1px solid #ddd;
    border-radius: 2px 2px 0 0;
  }
  .nav-tabs-justified > .active > a,
  .nav-tabs-justified > .active > a:hover,
  .nav-tabs-justified > .active > a:focus {
    border-bottom-color: #fff;
  }
}
.tab-content > .tab-pane {
  display: none;
}
.tab-content > .active {
  display: block;
}
.nav-tabs .dropdown-menu {
  margin-top: -1px;
  border-top-right-radius: 0;
  border-top-left-radius: 0;
}
.navbar {
  position: relative;
  min-height: 30px;
  margin-bottom: 18px;
  border: 1px solid transparent;
}
@media (min-width: 541px) {
  .navbar {
    border-radius: 2px;
  }
}
@media (min-width: 541px) {
  .navbar-header {
    float: left;
  }
}
.navbar-collapse {
  overflow-x: visible;
  padding-right: 0px;
  padding-left: 0px;
  border-top: 1px solid transparent;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.1);
  -webkit-overflow-scrolling: touch;
}
.navbar-collapse.in {
  overflow-y: auto;
}
@media (min-width: 541px) {
  .navbar-collapse {
    width: auto;
    border-top: 0;
    box-shadow: none;
  }
  .navbar-collapse.collapse {
    display: block !important;
    height: auto !important;
    padding-bottom: 0;
    overflow: visible !important;
  }
  .navbar-collapse.in {
    overflow-y: visible;
  }
  .navbar-fixed-top .navbar-collapse,
  .navbar-static-top .navbar-collapse,
  .navbar-fixed-bottom .navbar-collapse {
    padding-left: 0;
    padding-right: 0;
  }
}
.navbar-fixed-top .navbar-collapse,
.navbar-fixed-bottom .navbar-collapse {
  max-height: 340px;
}
@media (max-device-width: 540px) and (orientation: landscape) {
  .navbar-fixed-top .navbar-collapse,
  .navbar-fixed-bottom .navbar-collapse {
    max-height: 200px;
  }
}
.container > .navbar-header,
.container-fluid > .navbar-header,
.container > .navbar-collapse,
.container-fluid > .navbar-collapse {
  margin-right: 0px;
  margin-left: 0px;
}
@media (min-width: 541px) {
  .container > .navbar-header,
  .container-fluid > .navbar-header,
  .container > .navbar-collapse,
  .container-fluid > .navbar-collapse {
    margin-right: 0;
    margin-left: 0;
  }
}
.navbar-static-top {
  z-index: 1000;
  border-width: 0 0 1px;
}
@media (min-width: 541px) {
  .navbar-static-top {
    border-radius: 0;
  }
}
.navbar-fixed-top,
.navbar-fixed-bottom {
  position: fixed;
  right: 0;
  left: 0;
  z-index: 1030;
}
@media (min-width: 541px) {
  .navbar-fixed-top,
  .navbar-fixed-bottom {
    border-radius: 0;
  }
}
.navbar-fixed-top {
  top: 0;
  border-width: 0 0 1px;
}
.navbar-fixed-bottom {
  bottom: 0;
  margin-bottom: 0;
  border-width: 1px 0 0;
}
.navbar-brand {
  float: left;
  padding: 6px 0px;
  font-size: 17px;
  line-height: 18px;
  height: 30px;
}
.navbar-brand:hover,
.navbar-brand:focus {
  text-decoration: none;
}
.navbar-brand > img {
  display: block;
}
@media (min-width: 541px) {
  .navbar > .container .navbar-brand,
  .navbar > .container-fluid .navbar-brand {
    margin-left: 0px;
  }
}
.navbar-toggle {
  position: relative;
  float: right;
  margin-right: 0px;
  padding: 9px 10px;
  margin-top: -2px;
  margin-bottom: -2px;
  background-color: transparent;
  background-image: none;
  border: 1px solid transparent;
  border-radius: 2px;
}
.navbar-toggle:focus {
  outline: 0;
}
.navbar-toggle .icon-bar {
  display: block;
  width: 22px;
  height: 2px;
  border-radius: 1px;
}
.navbar-toggle .icon-bar + .icon-bar {
  margin-top: 4px;
}
@media (min-width: 541px) {
  .navbar-toggle {
    display: none;
  }
}
.navbar-nav {
  margin: 3px 0px;
}
.navbar-nav > li > a {
  padding-top: 10px;
  padding-bottom: 10px;
  line-height: 18px;
}
@media (max-width: 540px) {
  .navbar-nav .open .dropdown-menu {
    position: static;
    float: none;
    width: auto;
    margin-top: 0;
    background-color: transparent;
    border: 0;
    box-shadow: none;
  }
  .navbar-nav .open .dropdown-menu > li > a,
  .navbar-nav .open .dropdown-menu .dropdown-header {
    padding: 5px 15px 5px 25px;
  }
  .navbar-nav .open .dropdown-menu > li > a {
    line-height: 18px;
  }
  .navbar-nav .open .dropdown-menu > li > a:hover,
  .navbar-nav .open .dropdown-menu > li > a:focus {
    background-image: none;
  }
}
@media (min-width: 541px) {
  .navbar-nav {
    float: left;
    margin: 0;
  }
  .navbar-nav > li {
    float: left;
  }
  .navbar-nav > li > a {
    padding-top: 6px;
    padding-bottom: 6px;
  }
}
.navbar-form {
  margin-left: 0px;
  margin-right: 0px;
  padding: 10px 0px;
  border-top: 1px solid transparent;
  border-bottom: 1px solid transparent;
  -webkit-box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.1), 0 1px 0 rgba(255, 255, 255, 0.1);
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.1), 0 1px 0 rgba(255, 255, 255, 0.1);
  margin-top: -1px;
  margin-bottom: -1px;
}
@media (min-width: 768px) {
  .navbar-form .form-group {
    display: inline-block;
    margin-bottom: 0;
    vertical-align: middle;
  }
  .navbar-form .form-control {
    display: inline-block;
    width: auto;
    vertical-align: middle;
  }
  .navbar-form .form-control-static {
    display: inline-block;
  }
  .navbar-form .input-group {
    display: inline-table;
    vertical-align: middle;
  }
  .navbar-form .input-group .input-group-addon,
  .navbar-form .input-group .input-group-btn,
  .navbar-form .input-group .form-control {
    width: auto;
  }
  .navbar-form .input-group > .form-control {
    width: 100%;
  }
  .navbar-form .control-label {
    margin-bottom: 0;
    vertical-align: middle;
  }
  .navbar-form .radio,
  .navbar-form .checkbox {
    display: inline-block;
    margin-top: 0;
    margin-bottom: 0;
    vertical-align: middle;
  }
  .navbar-form .radio label,
  .navbar-form .checkbox label {
    padding-left: 0;
  }
  .navbar-form .radio input[type="radio"],
  .navbar-form .checkbox input[type="checkbox"] {
    position: relative;
    margin-left: 0;
  }
  .navbar-form .has-feedback .form-control-feedback {
    top: 0;
  }
}
@media (max-width: 540px) {
  .navbar-form .form-group {
    margin-bottom: 5px;
  }
  .navbar-form .form-group:last-child {
    margin-bottom: 0;
  }
}
@media (min-width: 541px) {
  .navbar-form {
    width: auto;
    border: 0;
    margin-left: 0;
    margin-right: 0;
    padding-top: 0;
    padding-bottom: 0;
    -webkit-box-shadow: none;
    box-shadow: none;
  }
}
.navbar-nav > li > .dropdown-menu {
  margin-top: 0;
  border-top-right-radius: 0;
  border-top-left-radius: 0;
}
.navbar-fixed-bottom .navbar-nav > li > .dropdown-menu {
  margin-bottom: 0;
  border-top-right-radius: 2px;
  border-top-left-radius: 2px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.navbar-btn {
  margin-top: -1px;
  margin-bottom: -1px;
}
.navbar-btn.btn-sm {
  margin-top: 0px;
  margin-bottom: 0px;
}
.navbar-btn.btn-xs {
  margin-top: 4px;
  margin-bottom: 4px;
}
.navbar-text {
  margin-top: 6px;
  margin-bottom: 6px;
}
@media (min-width: 541px) {
  .navbar-text {
    float: left;
    margin-left: 0px;
    margin-right: 0px;
  }
}
@media (min-width: 541px) {
  .navbar-left {
    float: left !important;
    float: left;
  }
  .navbar-right {
    float: right !important;
    float: right;
    margin-right: 0px;
  }
  .navbar-right ~ .navbar-right {
    margin-right: 0;
  }
}
.navbar-default {
  background-color: #f8f8f8;
  border-color: #e7e7e7;
}
.navbar-default .navbar-brand {
  color: #777;
}
.navbar-default .navbar-brand:hover,
.navbar-default .navbar-brand:focus {
  color: #5e5e5e;
  background-color: transparent;
}
.navbar-default .navbar-text {
  color: #777;
}
.navbar-default .navbar-nav > li > a {
  color: #777;
}
.navbar-default .navbar-nav > li > a:hover,
.navbar-default .navbar-nav > li > a:focus {
  color: #333;
  background-color: transparent;
}
.navbar-default .navbar-nav > .active > a,
.navbar-default .navbar-nav > .active > a:hover,
.navbar-default .navbar-nav > .active > a:focus {
  color: #555;
  background-color: #e7e7e7;
}
.navbar-default .navbar-nav > .disabled > a,
.navbar-default .navbar-nav > .disabled > a:hover,
.navbar-default .navbar-nav > .disabled > a:focus {
  color: #ccc;
  background-color: transparent;
}
.navbar-default .navbar-toggle {
  border-color: #ddd;
}
.navbar-default .navbar-toggle:hover,
.navbar-default .navbar-toggle:focus {
  background-color: #ddd;
}
.navbar-default .navbar-toggle .icon-bar {
  background-color: #888;
}
.navbar-default .navbar-collapse,
.navbar-default .navbar-form {
  border-color: #e7e7e7;
}
.navbar-default .navbar-nav > .open > a,
.navbar-default .navbar-nav > .open > a:hover,
.navbar-default .navbar-nav > .open > a:focus {
  background-color: #e7e7e7;
  color: #555;
}
@media (max-width: 540px) {
  .navbar-default .navbar-nav .open .dropdown-menu > li > a {
    color: #777;
  }
  .navbar-default .navbar-nav .open .dropdown-menu > li > a:hover,
  .navbar-default .navbar-nav .open .dropdown-menu > li > a:focus {
    color: #333;
    background-color: transparent;
  }
  .navbar-default .navbar-nav .open .dropdown-menu > .active > a,
  .navbar-default .navbar-nav .open .dropdown-menu > .active > a:hover,
  .navbar-default .navbar-nav .open .dropdown-menu > .active > a:focus {
    color: #555;
    background-color: #e7e7e7;
  }
  .navbar-default .navbar-nav .open .dropdown-menu > .disabled > a,
  .navbar-default .navbar-nav .open .dropdown-menu > .disabled > a:hover,
  .navbar-default .navbar-nav .open .dropdown-menu > .disabled > a:focus {
    color: #ccc;
    background-color: transparent;
  }
}
.navbar-default .navbar-link {
  color: #777;
}
.navbar-default .navbar-link:hover {
  color: #333;
}
.navbar-default .btn-link {
  color: #777;
}
.navbar-default .btn-link:hover,
.navbar-default .btn-link:focus {
  color: #333;
}
.navbar-default .btn-link[disabled]:hover,
fieldset[disabled] .navbar-default .btn-link:hover,
.navbar-default .btn-link[disabled]:focus,
fieldset[disabled] .navbar-default .btn-link:focus {
  color: #ccc;
}
.navbar-inverse {
  background-color: #222;
  border-color: #080808;
}
.navbar-inverse .navbar-brand {
  color: #9d9d9d;
}
.navbar-inverse .navbar-brand:hover,
.navbar-inverse .navbar-brand:focus {
  color: #fff;
  background-color: transparent;
}
.navbar-inverse .navbar-text {
  color: #9d9d9d;
}
.navbar-inverse .navbar-nav > li > a {
  color: #9d9d9d;
}
.navbar-inverse .navbar-nav > li > a:hover,
.navbar-inverse .navbar-nav > li > a:focus {
  color: #fff;
  background-color: transparent;
}
.navbar-inverse .navbar-nav > .active > a,
.navbar-inverse .navbar-nav > .active > a:hover,
.navbar-inverse .navbar-nav > .active > a:focus {
  color: #fff;
  background-color: #080808;
}
.navbar-inverse .navbar-nav > .disabled > a,
.navbar-inverse .navbar-nav > .disabled > a:hover,
.navbar-inverse .navbar-nav > .disabled > a:focus {
  color: #444;
  background-color: transparent;
}
.navbar-inverse .navbar-toggle {
  border-color: #333;
}
.navbar-inverse .navbar-toggle:hover,
.navbar-inverse .navbar-toggle:focus {
  background-color: #333;
}
.navbar-inverse .navbar-toggle .icon-bar {
  background-color: #fff;
}
.navbar-inverse .navbar-collapse,
.navbar-inverse .navbar-form {
  border-color: #101010;
}
.navbar-inverse .navbar-nav > .open > a,
.navbar-inverse .navbar-nav > .open > a:hover,
.navbar-inverse .navbar-nav > .open > a:focus {
  background-color: #080808;
  color: #fff;
}
@media (max-width: 540px) {
  .navbar-inverse .navbar-nav .open .dropdown-menu > .dropdown-header {
    border-color: #080808;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu .divider {
    background-color: #080808;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu > li > a {
    color: #9d9d9d;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu > li > a:hover,
  .navbar-inverse .navbar-nav .open .dropdown-menu > li > a:focus {
    color: #fff;
    background-color: transparent;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu > .active > a,
  .navbar-inverse .navbar-nav .open .dropdown-menu > .active > a:hover,
  .navbar-inverse .navbar-nav .open .dropdown-menu > .active > a:focus {
    color: #fff;
    background-color: #080808;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu > .disabled > a,
  .navbar-inverse .navbar-nav .open .dropdown-menu > .disabled > a:hover,
  .navbar-inverse .navbar-nav .open .dropdown-menu > .disabled > a:focus {
    color: #444;
    background-color: transparent;
  }
}
.navbar-inverse .navbar-link {
  color: #9d9d9d;
}
.navbar-inverse .navbar-link:hover {
  color: #fff;
}
.navbar-inverse .btn-link {
  color: #9d9d9d;
}
.navbar-inverse .btn-link:hover,
.navbar-inverse .btn-link:focus {
  color: #fff;
}
.navbar-inverse .btn-link[disabled]:hover,
fieldset[disabled] .navbar-inverse .btn-link:hover,
.navbar-inverse .btn-link[disabled]:focus,
fieldset[disabled] .navbar-inverse .btn-link:focus {
  color: #444;
}
.breadcrumb {
  padding: 8px 15px;
  margin-bottom: 18px;
  list-style: none;
  background-color: #f5f5f5;
  border-radius: 2px;
}
.breadcrumb > li {
  display: inline-block;
}
.breadcrumb > li + li:before {
  content: "/\00a0";
  padding: 0 5px;
  color: #5e5e5e;
}
.breadcrumb > .active {
  color: #777777;
}
.pagination {
  display: inline-block;
  padding-left: 0;
  margin: 18px 0;
  border-radius: 2px;
}
.pagination > li {
  display: inline;
}
.pagination > li > a,
.pagination > li > span {
  position: relative;
  float: left;
  padding: 6px 12px;
  line-height: 1.42857143;
  text-decoration: none;
  color: #337ab7;
  background-color: #fff;
  border: 1px solid #ddd;
  margin-left: -1px;
}
.pagination > li:first-child > a,
.pagination > li:first-child > span {
  margin-left: 0;
  border-bottom-left-radius: 2px;
  border-top-left-radius: 2px;
}
.pagination > li:last-child > a,
.pagination > li:last-child > span {
  border-bottom-right-radius: 2px;
  border-top-right-radius: 2px;
}
.pagination > li > a:hover,
.pagination > li > span:hover,
.pagination > li > a:focus,
.pagination > li > span:focus {
  z-index: 2;
  color: #23527c;
  background-color: #eeeeee;
  border-color: #ddd;
}
.pagination > .active > a,
.pagination > .active > span,
.pagination > .active > a:hover,
.pagination > .active > span:hover,
.pagination > .active > a:focus,
.pagination > .active > span:focus {
  z-index: 3;
  color: #fff;
  background-color: #337ab7;
  border-color: #337ab7;
  cursor: default;
}
.pagination > .disabled > span,
.pagination > .disabled > span:hover,
.pagination > .disabled > span:focus,
.pagination > .disabled > a,
.pagination > .disabled > a:hover,
.pagination > .disabled > a:focus {
  color: #777777;
  background-color: #fff;
  border-color: #ddd;
  cursor: not-allowed;
}
.pagination-lg > li > a,
.pagination-lg > li > span {
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
}
.pagination-lg > li:first-child > a,
.pagination-lg > li:first-child > span {
  border-bottom-left-radius: 3px;
  border-top-left-radius: 3px;
}
.pagination-lg > li:last-child > a,
.pagination-lg > li:last-child > span {
  border-bottom-right-radius: 3px;
  border-top-right-radius: 3px;
}
.pagination-sm > li > a,
.pagination-sm > li > span {
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
}
.pagination-sm > li:first-child > a,
.pagination-sm > li:first-child > span {
  border-bottom-left-radius: 1px;
  border-top-left-radius: 1px;
}
.pagination-sm > li:last-child > a,
.pagination-sm > li:last-child > span {
  border-bottom-right-radius: 1px;
  border-top-right-radius: 1px;
}
.pager {
  padding-left: 0;
  margin: 18px 0;
  list-style: none;
  text-align: center;
}
.pager li {
  display: inline;
}
.pager li > a,
.pager li > span {
  display: inline-block;
  padding: 5px 14px;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 15px;
}
.pager li > a:hover,
.pager li > a:focus {
  text-decoration: none;
  background-color: #eeeeee;
}
.pager .next > a,
.pager .next > span {
  float: right;
}
.pager .previous > a,
.pager .previous > span {
  float: left;
}
.pager .disabled > a,
.pager .disabled > a:hover,
.pager .disabled > a:focus,
.pager .disabled > span {
  color: #777777;
  background-color: #fff;
  cursor: not-allowed;
}
.label {
  display: inline;
  padding: .2em .6em .3em;
  font-size: 75%;
  font-weight: bold;
  line-height: 1;
  color: #fff;
  text-align: center;
  white-space: nowrap;
  vertical-align: baseline;
  border-radius: .25em;
}
a.label:hover,
a.label:focus {
  color: #fff;
  text-decoration: none;
  cursor: pointer;
}
.label:empty {
  display: none;
}
.btn .label {
  position: relative;
  top: -1px;
}
.label-default {
  background-color: #777777;
}
.label-default[href]:hover,
.label-default[href]:focus {
  background-color: #5e5e5e;
}
.label-primary {
  background-color: #337ab7;
}
.label-primary[href]:hover,
.label-primary[href]:focus {
  background-color: #286090;
}
.label-success {
  background-color: #5cb85c;
}
.label-success[href]:hover,
.label-success[href]:focus {
  background-color: #449d44;
}
.label-info {
  background-color: #5bc0de;
}
.label-info[href]:hover,
.label-info[href]:focus {
  background-color: #31b0d5;
}
.label-warning {
  background-color: #f0ad4e;
}
.label-warning[href]:hover,
.label-warning[href]:focus {
  background-color: #ec971f;
}
.label-danger {
  background-color: #d9534f;
}
.label-danger[href]:hover,
.label-danger[href]:focus {
  background-color: #c9302c;
}
.badge {
  display: inline-block;
  min-width: 10px;
  padding: 3px 7px;
  font-size: 12px;
  font-weight: bold;
  color: #fff;
  line-height: 1;
  vertical-align: middle;
  white-space: nowrap;
  text-align: center;
  background-color: #777777;
  border-radius: 10px;
}
.badge:empty {
  display: none;
}
.btn .badge {
  position: relative;
  top: -1px;
}
.btn-xs .badge,
.btn-group-xs > .btn .badge {
  top: 0;
  padding: 1px 5px;
}
a.badge:hover,
a.badge:focus {
  color: #fff;
  text-decoration: none;
  cursor: pointer;
}
.list-group-item.active > .badge,
.nav-pills > .active > a > .badge {
  color: #337ab7;
  background-color: #fff;
}
.list-group-item > .badge {
  float: right;
}
.list-group-item > .badge + .badge {
  margin-right: 5px;
}
.nav-pills > li > a > .badge {
  margin-left: 3px;
}
.jumbotron {
  padding-top: 30px;
  padding-bottom: 30px;
  margin-bottom: 30px;
  color: inherit;
  background-color: #eeeeee;
}
.jumbotron h1,
.jumbotron .h1 {
  color: inherit;
}
.jumbotron p {
  margin-bottom: 15px;
  font-size: 20px;
  font-weight: 200;
}
.jumbotron > hr {
  border-top-color: #d5d5d5;
}
.container .jumbotron,
.container-fluid .jumbotron {
  border-radius: 3px;
  padding-left: 0px;
  padding-right: 0px;
}
.jumbotron .container {
  max-width: 100%;
}
@media screen and (min-width: 768px) {
  .jumbotron {
    padding-top: 48px;
    padding-bottom: 48px;
  }
  .container .jumbotron,
  .container-fluid .jumbotron {
    padding-left: 60px;
    padding-right: 60px;
  }
  .jumbotron h1,
  .jumbotron .h1 {
    font-size: 59px;
  }
}
.thumbnail {
  display: block;
  padding: 4px;
  margin-bottom: 18px;
  line-height: 1.42857143;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 2px;
  -webkit-transition: border 0.2s ease-in-out;
  -o-transition: border 0.2s ease-in-out;
  transition: border 0.2s ease-in-out;
}
.thumbnail > img,
.thumbnail a > img {
  margin-left: auto;
  margin-right: auto;
}
a.thumbnail:hover,
a.thumbnail:focus,
a.thumbnail.active {
  border-color: #337ab7;
}
.thumbnail .caption {
  padding: 9px;
  color: #000;
}
.alert {
  padding: 15px;
  margin-bottom: 18px;
  border: 1px solid transparent;
  border-radius: 2px;
}
.alert h4 {
  margin-top: 0;
  color: inherit;
}
.alert .alert-link {
  font-weight: bold;
}
.alert > p,
.alert > ul {
  margin-bottom: 0;
}
.alert > p + p {
  margin-top: 5px;
}
.alert-dismissable,
.alert-dismissible {
  padding-right: 35px;
}
.alert-dismissable .close,
.alert-dismissible .close {
  position: relative;
  top: -2px;
  right: -21px;
  color: inherit;
}
.alert-success {
  background-color: #dff0d8;
  border-color: #d6e9c6;
  color: #3c763d;
}
.alert-success hr {
  border-top-color: #c9e2b3;
}
.alert-success .alert-link {
  color: #2b542c;
}
.alert-info {
  background-color: #d9edf7;
  border-color: #bce8f1;
  color: #31708f;
}
.alert-info hr {
  border-top-color: #a6e1ec;
}
.alert-info .alert-link {
  color: #245269;
}
.alert-warning {
  background-color: #fcf8e3;
  border-color: #faebcc;
  color: #8a6d3b;
}
.alert-warning hr {
  border-top-color: #f7e1b5;
}
.alert-warning .alert-link {
  color: #66512c;
}
.alert-danger {
  background-color: #f2dede;
  border-color: #ebccd1;
  color: #a94442;
}
.alert-danger hr {
  border-top-color: #e4b9c0;
}
.alert-danger .alert-link {
  color: #843534;
}
@-webkit-keyframes progress-bar-stripes {
  from {
    background-position: 40px 0;
  }
  to {
    background-position: 0 0;
  }
}
@keyframes progress-bar-stripes {
  from {
    background-position: 40px 0;
  }
  to {
    background-position: 0 0;
  }
}
.progress {
  overflow: hidden;
  height: 18px;
  margin-bottom: 18px;
  background-color: #f5f5f5;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.1);
  box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.1);
}
.progress-bar {
  float: left;
  width: 0%;
  height: 100%;
  font-size: 12px;
  line-height: 18px;
  color: #fff;
  text-align: center;
  background-color: #337ab7;
  -webkit-box-shadow: inset 0 -1px 0 rgba(0, 0, 0, 0.15);
  box-shadow: inset 0 -1px 0 rgba(0, 0, 0, 0.15);
  -webkit-transition: width 0.6s ease;
  -o-transition: width 0.6s ease;
  transition: width 0.6s ease;
}
.progress-striped .progress-bar,
.progress-bar-striped {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-size: 40px 40px;
}
.progress.active .progress-bar,
.progress-bar.active {
  -webkit-animation: progress-bar-stripes 2s linear infinite;
  -o-animation: progress-bar-stripes 2s linear infinite;
  animation: progress-bar-stripes 2s linear infinite;
}
.progress-bar-success {
  background-color: #5cb85c;
}
.progress-striped .progress-bar-success {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
}
.progress-bar-info {
  background-color: #5bc0de;
}
.progress-striped .progress-bar-info {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
}
.progress-bar-warning {
  background-color: #f0ad4e;
}
.progress-striped .progress-bar-warning {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
}
.progress-bar-danger {
  background-color: #d9534f;
}
.progress-striped .progress-bar-danger {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
}
.media {
  margin-top: 15px;
}
.media:first-child {
  margin-top: 0;
}
.media,
.media-body {
  zoom: 1;
  overflow: hidden;
}
.media-body {
  width: 10000px;
}
.media-object {
  display: block;
}
.media-object.img-thumbnail {
  max-width: none;
}
.media-right,
.media > .pull-right {
  padding-left: 10px;
}
.media-left,
.media > .pull-left {
  padding-right: 10px;
}
.media-left,
.media-right,
.media-body {
  display: table-cell;
  vertical-align: top;
}
.media-middle {
  vertical-align: middle;
}
.media-bottom {
  vertical-align: bottom;
}
.media-heading {
  margin-top: 0;
  margin-bottom: 5px;
}
.media-list {
  padding-left: 0;
  list-style: none;
}
.list-group {
  margin-bottom: 20px;
  padding-left: 0;
}
.list-group-item {
  position: relative;
  display: block;
  padding: 10px 15px;
  margin-bottom: -1px;
  background-color: #fff;
  border: 1px solid #ddd;
}
.list-group-item:first-child {
  border-top-right-radius: 2px;
  border-top-left-radius: 2px;
}
.list-group-item:last-child {
  margin-bottom: 0;
  border-bottom-right-radius: 2px;
  border-bottom-left-radius: 2px;
}
a.list-group-item,
button.list-group-item {
  color: #555;
}
a.list-group-item .list-group-item-heading,
button.list-group-item .list-group-item-heading {
  color: #333;
}
a.list-group-item:hover,
button.list-group-item:hover,
a.list-group-item:focus,
button.list-group-item:focus {
  text-decoration: none;
  color: #555;
  background-color: #f5f5f5;
}
button.list-group-item {
  width: 100%;
  text-align: left;
}
.list-group-item.disabled,
.list-group-item.disabled:hover,
.list-group-item.disabled:focus {
  background-color: #eeeeee;
  color: #777777;
  cursor: not-allowed;
}
.list-group-item.disabled .list-group-item-heading,
.list-group-item.disabled:hover .list-group-item-heading,
.list-group-item.disabled:focus .list-group-item-heading {
  color: inherit;
}
.list-group-item.disabled .list-group-item-text,
.list-group-item.disabled:hover .list-group-item-text,
.list-group-item.disabled:focus .list-group-item-text {
  color: #777777;
}
.list-group-item.active,
.list-group-item.active:hover,
.list-group-item.active:focus {
  z-index: 2;
  color: #fff;
  background-color: #337ab7;
  border-color: #337ab7;
}
.list-group-item.active .list-group-item-heading,
.list-group-item.active:hover .list-group-item-heading,
.list-group-item.active:focus .list-group-item-heading,
.list-group-item.active .list-group-item-heading > small,
.list-group-item.active:hover .list-group-item-heading > small,
.list-group-item.active:focus .list-group-item-heading > small,
.list-group-item.active .list-group-item-heading > .small,
.list-group-item.active:hover .list-group-item-heading > .small,
.list-group-item.active:focus .list-group-item-heading > .small {
  color: inherit;
}
.list-group-item.active .list-group-item-text,
.list-group-item.active:hover .list-group-item-text,
.list-group-item.active:focus .list-group-item-text {
  color: #c7ddef;
}
.list-group-item-success {
  color: #3c763d;
  background-color: #dff0d8;
}
a.list-group-item-success,
button.list-group-item-success {
  color: #3c763d;
}
a.list-group-item-success .list-group-item-heading,
button.list-group-item-success .list-group-item-heading {
  color: inherit;
}
a.list-group-item-success:hover,
button.list-group-item-success:hover,
a.list-group-item-success:focus,
button.list-group-item-success:focus {
  color: #3c763d;
  background-color: #d0e9c6;
}
a.list-group-item-success.active,
button.list-group-item-success.active,
a.list-group-item-success.active:hover,
button.list-group-item-success.active:hover,
a.list-group-item-success.active:focus,
button.list-group-item-success.active:focus {
  color: #fff;
  background-color: #3c763d;
  border-color: #3c763d;
}
.list-group-item-info {
  color: #31708f;
  background-color: #d9edf7;
}
a.list-group-item-info,
button.list-group-item-info {
  color: #31708f;
}
a.list-group-item-info .list-group-item-heading,
button.list-group-item-info .list-group-item-heading {
  color: inherit;
}
a.list-group-item-info:hover,
button.list-group-item-info:hover,
a.list-group-item-info:focus,
button.list-group-item-info:focus {
  color: #31708f;
  background-color: #c4e3f3;
}
a.list-group-item-info.active,
button.list-group-item-info.active,
a.list-group-item-info.active:hover,
button.list-group-item-info.active:hover,
a.list-group-item-info.active:focus,
button.list-group-item-info.active:focus {
  color: #fff;
  background-color: #31708f;
  border-color: #31708f;
}
.list-group-item-warning {
  color: #8a6d3b;
  background-color: #fcf8e3;
}
a.list-group-item-warning,
button.list-group-item-warning {
  color: #8a6d3b;
}
a.list-group-item-warning .list-group-item-heading,
button.list-group-item-warning .list-group-item-heading {
  color: inherit;
}
a.list-group-item-warning:hover,
button.list-group-item-warning:hover,
a.list-group-item-warning:focus,
button.list-group-item-warning:focus {
  color: #8a6d3b;
  background-color: #faf2cc;
}
a.list-group-item-warning.active,
button.list-group-item-warning.active,
a.list-group-item-warning.active:hover,
button.list-group-item-warning.active:hover,
a.list-group-item-warning.active:focus,
button.list-group-item-warning.active:focus {
  color: #fff;
  background-color: #8a6d3b;
  border-color: #8a6d3b;
}
.list-group-item-danger {
  color: #a94442;
  background-color: #f2dede;
}
a.list-group-item-danger,
button.list-group-item-danger {
  color: #a94442;
}
a.list-group-item-danger .list-group-item-heading,
button.list-group-item-danger .list-group-item-heading {
  color: inherit;
}
a.list-group-item-danger:hover,
button.list-group-item-danger:hover,
a.list-group-item-danger:focus,
button.list-group-item-danger:focus {
  color: #a94442;
  background-color: #ebcccc;
}
a.list-group-item-danger.active,
button.list-group-item-danger.active,
a.list-group-item-danger.active:hover,
button.list-group-item-danger.active:hover,
a.list-group-item-danger.active:focus,
button.list-group-item-danger.active:focus {
  color: #fff;
  background-color: #a94442;
  border-color: #a94442;
}
.list-group-item-heading {
  margin-top: 0;
  margin-bottom: 5px;
}
.list-group-item-text {
  margin-bottom: 0;
  line-height: 1.3;
}
.panel {
  margin-bottom: 18px;
  background-color: #fff;
  border: 1px solid transparent;
  border-radius: 2px;
  -webkit-box-shadow: 0 1px 1px rgba(0, 0, 0, 0.05);
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.05);
}
.panel-body {
  padding: 15px;
}
.panel-heading {
  padding: 10px 15px;
  border-bottom: 1px solid transparent;
  border-top-right-radius: 1px;
  border-top-left-radius: 1px;
}
.panel-heading > .dropdown .dropdown-toggle {
  color: inherit;
}
.panel-title {
  margin-top: 0;
  margin-bottom: 0;
  font-size: 15px;
  color: inherit;
}
.panel-title > a,
.panel-title > small,
.panel-title > .small,
.panel-title > small > a,
.panel-title > .small > a {
  color: inherit;
}
.panel-footer {
  padding: 10px 15px;
  background-color: #f5f5f5;
  border-top: 1px solid #ddd;
  border-bottom-right-radius: 1px;
  border-bottom-left-radius: 1px;
}
.panel > .list-group,
.panel > .panel-collapse > .list-group {
  margin-bottom: 0;
}
.panel > .list-group .list-group-item,
.panel > .panel-collapse > .list-group .list-group-item {
  border-width: 1px 0;
  border-radius: 0;
}
.panel > .list-group:first-child .list-group-item:first-child,
.panel > .panel-collapse > .list-group:first-child .list-group-item:first-child {
  border-top: 0;
  border-top-right-radius: 1px;
  border-top-left-radius: 1px;
}
.panel > .list-group:last-child .list-group-item:last-child,
.panel > .panel-collapse > .list-group:last-child .list-group-item:last-child {
  border-bottom: 0;
  border-bottom-right-radius: 1px;
  border-bottom-left-radius: 1px;
}
.panel > .panel-heading + .panel-collapse > .list-group .list-group-item:first-child {
  border-top-right-radius: 0;
  border-top-left-radius: 0;
}
.panel-heading + .list-group .list-group-item:first-child {
  border-top-width: 0;
}
.list-group + .panel-footer {
  border-top-width: 0;
}
.panel > .table,
.panel > .table-responsive > .table,
.panel > .panel-collapse > .table {
  margin-bottom: 0;
}
.panel > .table caption,
.panel > .table-responsive > .table caption,
.panel > .panel-collapse > .table caption {
  padding-left: 15px;
  padding-right: 15px;
}
.panel > .table:first-child,
.panel > .table-responsive:first-child > .table:first-child {
  border-top-right-radius: 1px;
  border-top-left-radius: 1px;
}
.panel > .table:first-child > thead:first-child > tr:first-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child,
.panel > .table:first-child > tbody:first-child > tr:first-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child {
  border-top-left-radius: 1px;
  border-top-right-radius: 1px;
}
.panel > .table:first-child > thead:first-child > tr:first-child td:first-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child td:first-child,
.panel > .table:first-child > tbody:first-child > tr:first-child td:first-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child td:first-child,
.panel > .table:first-child > thead:first-child > tr:first-child th:first-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child th:first-child,
.panel > .table:first-child > tbody:first-child > tr:first-child th:first-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child th:first-child {
  border-top-left-radius: 1px;
}
.panel > .table:first-child > thead:first-child > tr:first-child td:last-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child td:last-child,
.panel > .table:first-child > tbody:first-child > tr:first-child td:last-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child td:last-child,
.panel > .table:first-child > thead:first-child > tr:first-child th:last-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child th:last-child,
.panel > .table:first-child > tbody:first-child > tr:first-child th:last-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child th:last-child {
  border-top-right-radius: 1px;
}
.panel > .table:last-child,
.panel > .table-responsive:last-child > .table:last-child {
  border-bottom-right-radius: 1px;
  border-bottom-left-radius: 1px;
}
.panel > .table:last-child > tbody:last-child > tr:last-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child {
  border-bottom-left-radius: 1px;
  border-bottom-right-radius: 1px;
}
.panel > .table:last-child > tbody:last-child > tr:last-child td:first-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child td:first-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child td:first-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child td:first-child,
.panel > .table:last-child > tbody:last-child > tr:last-child th:first-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child th:first-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child th:first-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child th:first-child {
  border-bottom-left-radius: 1px;
}
.panel > .table:last-child > tbody:last-child > tr:last-child td:last-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child td:last-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child td:last-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child td:last-child,
.panel > .table:last-child > tbody:last-child > tr:last-child th:last-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child th:last-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child th:last-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child th:last-child {
  border-bottom-right-radius: 1px;
}
.panel > .panel-body + .table,
.panel > .panel-body + .table-responsive,
.panel > .table + .panel-body,
.panel > .table-responsive + .panel-body {
  border-top: 1px solid #ddd;
}
.panel > .table > tbody:first-child > tr:first-child th,
.panel > .table > tbody:first-child > tr:first-child td {
  border-top: 0;
}
.panel > .table-bordered,
.panel > .table-responsive > .table-bordered {
  border: 0;
}
.panel > .table-bordered > thead > tr > th:first-child,
.panel > .table-responsive > .table-bordered > thead > tr > th:first-child,
.panel > .table-bordered > tbody > tr > th:first-child,
.panel > .table-responsive > .table-bordered > tbody > tr > th:first-child,
.panel > .table-bordered > tfoot > tr > th:first-child,
.panel > .table-responsive > .table-bordered > tfoot > tr > th:first-child,
.panel > .table-bordered > thead > tr > td:first-child,
.panel > .table-responsive > .table-bordered > thead > tr > td:first-child,
.panel > .table-bordered > tbody > tr > td:first-child,
.panel > .table-responsive > .table-bordered > tbody > tr > td:first-child,
.panel > .table-bordered > tfoot > tr > td:first-child,
.panel > .table-responsive > .table-bordered > tfoot > tr > td:first-child {
  border-left: 0;
}
.panel > .table-bordered > thead > tr > th:last-child,
.panel > .table-responsive > .table-bordered > thead > tr > th:last-child,
.panel > .table-bordered > tbody > tr > th:last-child,
.panel > .table-responsive > .table-bordered > tbody > tr > th:last-child,
.panel > .table-bordered > tfoot > tr > th:last-child,
.panel > .table-responsive > .table-bordered > tfoot > tr > th:last-child,
.panel > .table-bordered > thead > tr > td:last-child,
.panel > .table-responsive > .table-bordered > thead > tr > td:last-child,
.panel > .table-bordered > tbody > tr > td:last-child,
.panel > .table-responsive > .table-bordered > tbody > tr > td:last-child,
.panel > .table-bordered > tfoot > tr > td:last-child,
.panel > .table-responsive > .table-bordered > tfoot > tr > td:last-child {
  border-right: 0;
}
.panel > .table-bordered > thead > tr:first-child > td,
.panel > .table-responsive > .table-bordered > thead > tr:first-child > td,
.panel > .table-bordered > tbody > tr:first-child > td,
.panel > .table-responsive > .table-bordered > tbody > tr:first-child > td,
.panel > .table-bordered > thead > tr:first-child > th,
.panel > .table-responsive > .table-bordered > thead > tr:first-child > th,
.panel > .table-bordered > tbody > tr:first-child > th,
.panel > .table-responsive > .table-bordered > tbody > tr:first-child > th {
  border-bottom: 0;
}
.panel > .table-bordered > tbody > tr:last-child > td,
.panel > .table-responsive > .table-bordered > tbody > tr:last-child > td,
.panel > .table-bordered > tfoot > tr:last-child > td,
.panel > .table-responsive > .table-bordered > tfoot > tr:last-child > td,
.panel > .table-bordered > tbody > tr:last-child > th,
.panel > .table-responsive > .table-bordered > tbody > tr:last-child > th,
.panel > .table-bordered > tfoot > tr:last-child > th,
.panel > .table-responsive > .table-bordered > tfoot > tr:last-child > th {
  border-bottom: 0;
}
.panel > .table-responsive {
  border: 0;
  margin-bottom: 0;
}
.panel-group {
  margin-bottom: 18px;
}
.panel-group .panel {
  margin-bottom: 0;
  border-radius: 2px;
}
.panel-group .panel + .panel {
  margin-top: 5px;
}
.panel-group .panel-heading {
  border-bottom: 0;
}
.panel-group .panel-heading + .panel-collapse > .panel-body,
.panel-group .panel-heading + .panel-collapse > .list-group {
  border-top: 1px solid #ddd;
}
.panel-group .panel-footer {
  border-top: 0;
}
.panel-group .panel-footer + .panel-collapse .panel-body {
  border-bottom: 1px solid #ddd;
}
.panel-default {
  border-color: #ddd;
}
.panel-default > .panel-heading {
  color: #333333;
  background-color: #f5f5f5;
  border-color: #ddd;
}
.panel-default > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #ddd;
}
.panel-default > .panel-heading .badge {
  color: #f5f5f5;
  background-color: #333333;
}
.panel-default > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #ddd;
}
.panel-primary {
  border-color: #337ab7;
}
.panel-primary > .panel-heading {
  color: #fff;
  background-color: #337ab7;
  border-color: #337ab7;
}
.panel-primary > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #337ab7;
}
.panel-primary > .panel-heading .badge {
  color: #337ab7;
  background-color: #fff;
}
.panel-primary > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #337ab7;
}
.panel-success {
  border-color: #d6e9c6;
}
.panel-success > .panel-heading {
  color: #3c763d;
  background-color: #dff0d8;
  border-color: #d6e9c6;
}
.panel-success > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #d6e9c6;
}
.panel-success > .panel-heading .badge {
  color: #dff0d8;
  background-color: #3c763d;
}
.panel-success > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #d6e9c6;
}
.panel-info {
  border-color: #bce8f1;
}
.panel-info > .panel-heading {
  color: #31708f;
  background-color: #d9edf7;
  border-color: #bce8f1;
}
.panel-info > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #bce8f1;
}
.panel-info > .panel-heading .badge {
  color: #d9edf7;
  background-color: #31708f;
}
.panel-info > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #bce8f1;
}
.panel-warning {
  border-color: #faebcc;
}
.panel-warning > .panel-heading {
  color: #8a6d3b;
  background-color: #fcf8e3;
  border-color: #faebcc;
}
.panel-warning > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #faebcc;
}
.panel-warning > .panel-heading .badge {
  color: #fcf8e3;
  background-color: #8a6d3b;
}
.panel-warning > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #faebcc;
}
.panel-danger {
  border-color: #ebccd1;
}
.panel-danger > .panel-heading {
  color: #a94442;
  background-color: #f2dede;
  border-color: #ebccd1;
}
.panel-danger > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #ebccd1;
}
.panel-danger > .panel-heading .badge {
  color: #f2dede;
  background-color: #a94442;
}
.panel-danger > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #ebccd1;
}
.embed-responsive {
  position: relative;
  display: block;
  height: 0;
  padding: 0;
  overflow: hidden;
}
.embed-responsive .embed-responsive-item,
.embed-responsive iframe,
.embed-responsive embed,
.embed-responsive object,
.embed-responsive video {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  height: 100%;
  width: 100%;
  border: 0;
}
.embed-responsive-16by9 {
  padding-bottom: 56.25%;
}
.embed-responsive-4by3 {
  padding-bottom: 75%;
}
.well {
  min-height: 20px;
  padding: 19px;
  margin-bottom: 20px;
  background-color: #f5f5f5;
  border: 1px solid #e3e3e3;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.05);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.05);
}
.well blockquote {
  border-color: #ddd;
  border-color: rgba(0, 0, 0, 0.15);
}
.well-lg {
  padding: 24px;
  border-radius: 3px;
}
.well-sm {
  padding: 9px;
  border-radius: 1px;
}
.close {
  float: right;
  font-size: 19.5px;
  font-weight: bold;
  line-height: 1;
  color: #000;
  text-shadow: 0 1px 0 #fff;
  opacity: 0.2;
  filter: alpha(opacity=20);
}
.close:hover,
.close:focus {
  color: #000;
  text-decoration: none;
  cursor: pointer;
  opacity: 0.5;
  filter: alpha(opacity=50);
}
button.close {
  padding: 0;
  cursor: pointer;
  background: transparent;
  border: 0;
  -webkit-appearance: none;
}
.modal-open {
  overflow: hidden;
}
.modal {
  display: none;
  overflow: hidden;
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 1050;
  -webkit-overflow-scrolling: touch;
  outline: 0;
}
.modal.fade .modal-dialog {
  -webkit-transform: translate(0, -25%);
  -ms-transform: translate(0, -25%);
  -o-transform: translate(0, -25%);
  transform: translate(0, -25%);
  -webkit-transition: -webkit-transform 0.3s ease-out;
  -moz-transition: -moz-transform 0.3s ease-out;
  -o-transition: -o-transform 0.3s ease-out;
  transition: transform 0.3s ease-out;
}
.modal.in .modal-dialog {
  -webkit-transform: translate(0, 0);
  -ms-transform: translate(0, 0);
  -o-transform: translate(0, 0);
  transform: translate(0, 0);
}
.modal-open .modal {
  overflow-x: hidden;
  overflow-y: auto;
}
.modal-dialog {
  position: relative;
  width: auto;
  margin: 10px;
}
.modal-content {
  position: relative;
  background-color: #fff;
  border: 1px solid #999;
  border: 1px solid rgba(0, 0, 0, 0.2);
  border-radius: 3px;
  -webkit-box-shadow: 0 3px 9px rgba(0, 0, 0, 0.5);
  box-shadow: 0 3px 9px rgba(0, 0, 0, 0.5);
  background-clip: padding-box;
  outline: 0;
}
.modal-backdrop {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 1040;
  background-color: #000;
}
.modal-backdrop.fade {
  opacity: 0;
  filter: alpha(opacity=0);
}
.modal-backdrop.in {
  opacity: 0.5;
  filter: alpha(opacity=50);
}
.modal-header {
  padding: 15px;
  border-bottom: 1px solid #e5e5e5;
}
.modal-header .close {
  margin-top: -2px;
}
.modal-title {
  margin: 0;
  line-height: 1.42857143;
}
.modal-body {
  position: relative;
  padding: 15px;
}
.modal-footer {
  padding: 15px;
  text-align: right;
  border-top: 1px solid #e5e5e5;
}
.modal-footer .btn + .btn {
  margin-left: 5px;
  margin-bottom: 0;
}
.modal-footer .btn-group .btn + .btn {
  margin-left: -1px;
}
.modal-footer .btn-block + .btn-block {
  margin-left: 0;
}
.modal-scrollbar-measure {
  position: absolute;
  top: -9999px;
  width: 50px;
  height: 50px;
  overflow: scroll;
}
@media (min-width: 768px) {
  .modal-dialog {
    width: 600px;
    margin: 30px auto;
  }
  .modal-content {
    -webkit-box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
  }
  .modal-sm {
    width: 300px;
  }
}
@media (min-width: 992px) {
  .modal-lg {
    width: 900px;
  }
}
.tooltip {
  position: absolute;
  z-index: 1070;
  display: block;
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-style: normal;
  font-weight: normal;
  letter-spacing: normal;
  line-break: auto;
  line-height: 1.42857143;
  text-align: left;
  text-align: start;
  text-decoration: none;
  text-shadow: none;
  text-transform: none;
  white-space: normal;
  word-break: normal;
  word-spacing: normal;
  word-wrap: normal;
  font-size: 12px;
  opacity: 0;
  filter: alpha(opacity=0);
}
.tooltip.in {
  opacity: 0.9;
  filter: alpha(opacity=90);
}
.tooltip.top {
  margin-top: -3px;
  padding: 5px 0;
}
.tooltip.right {
  margin-left: 3px;
  padding: 0 5px;
}
.tooltip.bottom {
  margin-top: 3px;
  padding: 5px 0;
}
.tooltip.left {
  margin-left: -3px;
  padding: 0 5px;
}
.tooltip-inner {
  max-width: 200px;
  padding: 3px 8px;
  color: #fff;
  text-align: center;
  background-color: #000;
  border-radius: 2px;
}
.tooltip-arrow {
  position: absolute;
  width: 0;
  height: 0;
  border-color: transparent;
  border-style: solid;
}
.tooltip.top .tooltip-arrow {
  bottom: 0;
  left: 50%;
  margin-left: -5px;
  border-width: 5px 5px 0;
  border-top-color: #000;
}
.tooltip.top-left .tooltip-arrow {
  bottom: 0;
  right: 5px;
  margin-bottom: -5px;
  border-width: 5px 5px 0;
  border-top-color: #000;
}
.tooltip.top-right .tooltip-arrow {
  bottom: 0;
  left: 5px;
  margin-bottom: -5px;
  border-width: 5px 5px 0;
  border-top-color: #000;
}
.tooltip.right .tooltip-arrow {
  top: 50%;
  left: 0;
  margin-top: -5px;
  border-width: 5px 5px 5px 0;
  border-right-color: #000;
}
.tooltip.left .tooltip-arrow {
  top: 50%;
  right: 0;
  margin-top: -5px;
  border-width: 5px 0 5px 5px;
  border-left-color: #000;
}
.tooltip.bottom .tooltip-arrow {
  top: 0;
  left: 50%;
  margin-left: -5px;
  border-width: 0 5px 5px;
  border-bottom-color: #000;
}
.tooltip.bottom-left .tooltip-arrow {
  top: 0;
  right: 5px;
  margin-top: -5px;
  border-width: 0 5px 5px;
  border-bottom-color: #000;
}
.tooltip.bottom-right .tooltip-arrow {
  top: 0;
  left: 5px;
  margin-top: -5px;
  border-width: 0 5px 5px;
  border-bottom-color: #000;
}
.popover {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 1060;
  display: none;
  max-width: 276px;
  padding: 1px;
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-style: normal;
  font-weight: normal;
  letter-spacing: normal;
  line-break: auto;
  line-height: 1.42857143;
  text-align: left;
  text-align: start;
  text-decoration: none;
  text-shadow: none;
  text-transform: none;
  white-space: normal;
  word-break: normal;
  word-spacing: normal;
  word-wrap: normal;
  font-size: 13px;
  background-color: #fff;
  background-clip: padding-box;
  border: 1px solid #ccc;
  border: 1px solid rgba(0, 0, 0, 0.2);
  border-radius: 3px;
  -webkit-box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
}
.popover.top {
  margin-top: -10px;
}
.popover.right {
  margin-left: 10px;
}
.popover.bottom {
  margin-top: 10px;
}
.popover.left {
  margin-left: -10px;
}
.popover-title {
  margin: 0;
  padding: 8px 14px;
  font-size: 13px;
  background-color: #f7f7f7;
  border-bottom: 1px solid #ebebeb;
  border-radius: 2px 2px 0 0;
}
.popover-content {
  padding: 9px 14px;
}
.popover > .arrow,
.popover > .arrow:after {
  position: absolute;
  display: block;
  width: 0;
  height: 0;
  border-color: transparent;
  border-style: solid;
}
.popover > .arrow {
  border-width: 11px;
}
.popover > .arrow:after {
  border-width: 10px;
  content: "";
}
.popover.top > .arrow {
  left: 50%;
  margin-left: -11px;
  border-bottom-width: 0;
  border-top-color: #999999;
  border-top-color: rgba(0, 0, 0, 0.25);
  bottom: -11px;
}
.popover.top > .arrow:after {
  content: " ";
  bottom: 1px;
  margin-left: -10px;
  border-bottom-width: 0;
  border-top-color: #fff;
}
.popover.right > .arrow {
  top: 50%;
  left: -11px;
  margin-top: -11px;
  border-left-width: 0;
  border-right-color: #999999;
  border-right-color: rgba(0, 0, 0, 0.25);
}
.popover.right > .arrow:after {
  content: " ";
  left: 1px;
  bottom: -10px;
  border-left-width: 0;
  border-right-color: #fff;
}
.popover.bottom > .arrow {
  left: 50%;
  margin-left: -11px;
  border-top-width: 0;
  border-bottom-color: #999999;
  border-bottom-color: rgba(0, 0, 0, 0.25);
  top: -11px;
}
.popover.bottom > .arrow:after {
  content: " ";
  top: 1px;
  margin-left: -10px;
  border-top-width: 0;
  border-bottom-color: #fff;
}
.popover.left > .arrow {
  top: 50%;
  right: -11px;
  margin-top: -11px;
  border-right-width: 0;
  border-left-color: #999999;
  border-left-color: rgba(0, 0, 0, 0.25);
}
.popover.left > .arrow:after {
  content: " ";
  right: 1px;
  border-right-width: 0;
  border-left-color: #fff;
  bottom: -10px;
}
.carousel {
  position: relative;
}
.carousel-inner {
  position: relative;
  overflow: hidden;
  width: 100%;
}
.carousel-inner > .item {
  display: none;
  position: relative;
  -webkit-transition: 0.6s ease-in-out left;
  -o-transition: 0.6s ease-in-out left;
  transition: 0.6s ease-in-out left;
}
.carousel-inner > .item > img,
.carousel-inner > .item > a > img {
  line-height: 1;
}
@media all and (transform-3d), (-webkit-transform-3d) {
  .carousel-inner > .item {
    -webkit-transition: -webkit-transform 0.6s ease-in-out;
    -moz-transition: -moz-transform 0.6s ease-in-out;
    -o-transition: -o-transform 0.6s ease-in-out;
    transition: transform 0.6s ease-in-out;
    -webkit-backface-visibility: hidden;
    -moz-backface-visibility: hidden;
    backface-visibility: hidden;
    -webkit-perspective: 1000px;
    -moz-perspective: 1000px;
    perspective: 1000px;
  }
  .carousel-inner > .item.next,
  .carousel-inner > .item.active.right {
    -webkit-transform: translate3d(100%, 0, 0);
    transform: translate3d(100%, 0, 0);
    left: 0;
  }
  .carousel-inner > .item.prev,
  .carousel-inner > .item.active.left {
    -webkit-transform: translate3d(-100%, 0, 0);
    transform: translate3d(-100%, 0, 0);
    left: 0;
  }
  .carousel-inner > .item.next.left,
  .carousel-inner > .item.prev.right,
  .carousel-inner > .item.active {
    -webkit-transform: translate3d(0, 0, 0);
    transform: translate3d(0, 0, 0);
    left: 0;
  }
}
.carousel-inner > .active,
.carousel-inner > .next,
.carousel-inner > .prev {
  display: block;
}
.carousel-inner > .active {
  left: 0;
}
.carousel-inner > .next,
.carousel-inner > .prev {
  position: absolute;
  top: 0;
  width: 100%;
}
.carousel-inner > .next {
  left: 100%;
}
.carousel-inner > .prev {
  left: -100%;
}
.carousel-inner > .next.left,
.carousel-inner > .prev.right {
  left: 0;
}
.carousel-inner > .active.left {
  left: -100%;
}
.carousel-inner > .active.right {
  left: 100%;
}
.carousel-control {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  width: 15%;
  opacity: 0.5;
  filter: alpha(opacity=50);
  font-size: 20px;
  color: #fff;
  text-align: center;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.6);
  background-color: rgba(0, 0, 0, 0);
}
.carousel-control.left {
  background-image: -webkit-linear-gradient(left, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0.0001) 100%);
  background-image: -o-linear-gradient(left, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0.0001) 100%);
  background-image: linear-gradient(to right, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0.0001) 100%);
  background-repeat: repeat-x;
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#80000000', endColorstr='#00000000', GradientType=1);
}
.carousel-control.right {
  left: auto;
  right: 0;
  background-image: -webkit-linear-gradient(left, rgba(0, 0, 0, 0.0001) 0%, rgba(0, 0, 0, 0.5) 100%);
  background-image: -o-linear-gradient(left, rgba(0, 0, 0, 0.0001) 0%, rgba(0, 0, 0, 0.5) 100%);
  background-image: linear-gradient(to right, rgba(0, 0, 0, 0.0001) 0%, rgba(0, 0, 0, 0.5) 100%);
  background-repeat: repeat-x;
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#00000000', endColorstr='#80000000', GradientType=1);
}
.carousel-control:hover,
.carousel-control:focus {
  outline: 0;
  color: #fff;
  text-decoration: none;
  opacity: 0.9;
  filter: alpha(opacity=90);
}
.carousel-control .icon-prev,
.carousel-control .icon-next,
.carousel-control .glyphicon-chevron-left,
.carousel-control .glyphicon-chevron-right {
  position: absolute;
  top: 50%;
  margin-top: -10px;
  z-index: 5;
  display: inline-block;
}
.carousel-control .icon-prev,
.carousel-control .glyphicon-chevron-left {
  left: 50%;
  margin-left: -10px;
}
.carousel-control .icon-next,
.carousel-control .glyphicon-chevron-right {
  right: 50%;
  margin-right: -10px;
}
.carousel-control .icon-prev,
.carousel-control .icon-next {
  width: 20px;
  height: 20px;
  line-height: 1;
  font-family: serif;
}
.carousel-control .icon-prev:before {
  content: '\2039';
}
.carousel-control .icon-next:before {
  content: '\203a';
}
.carousel-indicators {
  position: absolute;
  bottom: 10px;
  left: 50%;
  z-index: 15;
  width: 60%;
  margin-left: -30%;
  padding-left: 0;
  list-style: none;
  text-align: center;
}
.carousel-indicators li {
  display: inline-block;
  width: 10px;
  height: 10px;
  margin: 1px;
  text-indent: -999px;
  border: 1px solid #fff;
  border-radius: 10px;
  cursor: pointer;
  background-color: #000 \9;
  background-color: rgba(0, 0, 0, 0);
}
.carousel-indicators .active {
  margin: 0;
  width: 12px;
  height: 12px;
  background-color: #fff;
}
.carousel-caption {
  position: absolute;
  left: 15%;
  right: 15%;
  bottom: 20px;
  z-index: 10;
  padding-top: 20px;
  padding-bottom: 20px;
  color: #fff;
  text-align: center;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.6);
}
.carousel-caption .btn {
  text-shadow: none;
}
@media screen and (min-width: 768px) {
  .carousel-control .glyphicon-chevron-left,
  .carousel-control .glyphicon-chevron-right,
  .carousel-control .icon-prev,
  .carousel-control .icon-next {
    width: 30px;
    height: 30px;
    margin-top: -10px;
    font-size: 30px;
  }
  .carousel-control .glyphicon-chevron-left,
  .carousel-control .icon-prev {
    margin-left: -10px;
  }
  .carousel-control .glyphicon-chevron-right,
  .carousel-control .icon-next {
    margin-right: -10px;
  }
  .carousel-caption {
    left: 20%;
    right: 20%;
    padding-bottom: 30px;
  }
  .carousel-indicators {
    bottom: 20px;
  }
}
.clearfix:before,
.clearfix:after,
.dl-horizontal dd:before,
.dl-horizontal dd:after,
.container:before,
.container:after,
.container-fluid:before,
.container-fluid:after,
.row:before,
.row:after,
.form-horizontal .form-group:before,
.form-horizontal .form-group:after,
.btn-toolbar:before,
.btn-toolbar:after,
.btn-group-vertical > .btn-group:before,
.btn-group-vertical > .btn-group:after,
.nav:before,
.nav:after,
.navbar:before,
.navbar:after,
.navbar-header:before,
.navbar-header:after,
.navbar-collapse:before,
.navbar-collapse:after,
.pager:before,
.pager:after,
.panel-body:before,
.panel-body:after,
.modal-header:before,
.modal-header:after,
.modal-footer:before,
.modal-footer:after,
.item_buttons:before,
.item_buttons:after {
  content: " ";
  display: table;
}
.clearfix:after,
.dl-horizontal dd:after,
.container:after,
.container-fluid:after,
.row:after,
.form-horizontal .form-group:after,
.btn-toolbar:after,
.btn-group-vertical > .btn-group:after,
.nav:after,
.navbar:after,
.navbar-header:after,
.navbar-collapse:after,
.pager:after,
.panel-body:after,
.modal-header:after,
.modal-footer:after,
.item_buttons:after {
  clear: both;
}
.center-block {
  display: block;
  margin-left: auto;
  margin-right: auto;
}
.pull-right {
  float: right !important;
}
.pull-left {
  float: left !important;
}
.hide {
  display: none !important;
}
.show {
  display: block !important;
}
.invisible {
  visibility: hidden;
}
.text-hide {
  font: 0/0 a;
  color: transparent;
  text-shadow: none;
  background-color: transparent;
  border: 0;
}
.hidden {
  display: none !important;
}
.affix {
  position: fixed;
}
@-ms-viewport {
  width: device-width;
}
.visible-xs,
.visible-sm,
.visible-md,
.visible-lg {
  display: none !important;
}
.visible-xs-block,
.visible-xs-inline,
.visible-xs-inline-block,
.visible-sm-block,
.visible-sm-inline,
.visible-sm-inline-block,
.visible-md-block,
.visible-md-inline,
.visible-md-inline-block,
.visible-lg-block,
.visible-lg-inline,
.visible-lg-inline-block {
  display: none !important;
}
@media (max-width: 767px) {
  .visible-xs {
    display: block !important;
  }
  table.visible-xs {
    display: table !important;
  }
  tr.visible-xs {
    display: table-row !important;
  }
  th.visible-xs,
  td.visible-xs {
    display: table-cell !important;
  }
}
@media (max-width: 767px) {
  .visible-xs-block {
    display: block !important;
  }
}
@media (max-width: 767px) {
  .visible-xs-inline {
    display: inline !important;
  }
}
@media (max-width: 767px) {
  .visible-xs-inline-block {
    display: inline-block !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .visible-sm {
    display: block !important;
  }
  table.visible-sm {
    display: table !important;
  }
  tr.visible-sm {
    display: table-row !important;
  }
  th.visible-sm,
  td.visible-sm {
    display: table-cell !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .visible-sm-block {
    display: block !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .visible-sm-inline {
    display: inline !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .visible-sm-inline-block {
    display: inline-block !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .visible-md {
    display: block !important;
  }
  table.visible-md {
    display: table !important;
  }
  tr.visible-md {
    display: table-row !important;
  }
  th.visible-md,
  td.visible-md {
    display: table-cell !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .visible-md-block {
    display: block !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .visible-md-inline {
    display: inline !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .visible-md-inline-block {
    display: inline-block !important;
  }
}
@media (min-width: 1200px) {
  .visible-lg {
    display: block !important;
  }
  table.visible-lg {
    display: table !important;
  }
  tr.visible-lg {
    display: table-row !important;
  }
  th.visible-lg,
  td.visible-lg {
    display: table-cell !important;
  }
}
@media (min-width: 1200px) {
  .visible-lg-block {
    display: block !important;
  }
}
@media (min-width: 1200px) {
  .visible-lg-inline {
    display: inline !important;
  }
}
@media (min-width: 1200px) {
  .visible-lg-inline-block {
    display: inline-block !important;
  }
}
@media (max-width: 767px) {
  .hidden-xs {
    display: none !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .hidden-sm {
    display: none !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .hidden-md {
    display: none !important;
  }
}
@media (min-width: 1200px) {
  .hidden-lg {
    display: none !important;
  }
}
.visible-print {
  display: none !important;
}
@media print {
  .visible-print {
    display: block !important;
  }
  table.visible-print {
    display: table !important;
  }
  tr.visible-print {
    display: table-row !important;
  }
  th.visible-print,
  td.visible-print {
    display: table-cell !important;
  }
}
.visible-print-block {
  display: none !important;
}
@media print {
  .visible-print-block {
    display: block !important;
  }
}
.visible-print-inline {
  display: none !important;
}
@media print {
  .visible-print-inline {
    display: inline !important;
  }
}
.visible-print-inline-block {
  display: none !important;
}
@media print {
  .visible-print-inline-block {
    display: inline-block !important;
  }
}
@media print {
  .hidden-print {
    display: none !important;
  }
}
/*!
*
* Font Awesome
*
*/
/*!
 *  Font Awesome 4.7.0 by @davegandy - http://fontawesome.io - @fontawesome
 *  License - http://fontawesome.io/license (Font: SIL OFL 1.1, CSS: MIT License)
 */
/* FONT PATH
 * -------------------------- */
@font-face {
  font-family: 'FontAwesome';
  src: url('../components/font-awesome/fonts/fontawesome-webfont.eot?v=4.7.0');
  src: url('../components/font-awesome/fonts/fontawesome-webfont.eot?#iefix&v=4.7.0') format('embedded-opentype'), url('../components/font-awesome/fonts/fontawesome-webfont.woff2?v=4.7.0') format('woff2'), url('../components/font-awesome/fonts/fontawesome-webfont.woff?v=4.7.0') format('woff'), url('../components/font-awesome/fonts/fontawesome-webfont.ttf?v=4.7.0') format('truetype'), url('../components/font-awesome/fonts/fontawesome-webfont.svg?v=4.7.0#fontawesomeregular') format('svg');
  font-weight: normal;
  font-style: normal;
}
.fa {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
/* makes the font 33% larger relative to the icon container */
.fa-lg {
  font-size: 1.33333333em;
  line-height: 0.75em;
  vertical-align: -15%;
}
.fa-2x {
  font-size: 2em;
}
.fa-3x {
  font-size: 3em;
}
.fa-4x {
  font-size: 4em;
}
.fa-5x {
  font-size: 5em;
}
.fa-fw {
  width: 1.28571429em;
  text-align: center;
}
.fa-ul {
  padding-left: 0;
  margin-left: 2.14285714em;
  list-style-type: none;
}
.fa-ul > li {
  position: relative;
}
.fa-li {
  position: absolute;
  left: -2.14285714em;
  width: 2.14285714em;
  top: 0.14285714em;
  text-align: center;
}
.fa-li.fa-lg {
  left: -1.85714286em;
}
.fa-border {
  padding: .2em .25em .15em;
  border: solid 0.08em #eee;
  border-radius: .1em;
}
.fa-pull-left {
  float: left;
}
.fa-pull-right {
  float: right;
}
.fa.fa-pull-left {
  margin-right: .3em;
}
.fa.fa-pull-right {
  margin-left: .3em;
}
/* Deprecated as of 4.4.0 */
.pull-right {
  float: right;
}
.pull-left {
  float: left;
}
.fa.pull-left {
  margin-right: .3em;
}
.fa.pull-right {
  margin-left: .3em;
}
.fa-spin {
  -webkit-animation: fa-spin 2s infinite linear;
  animation: fa-spin 2s infinite linear;
}
.fa-pulse {
  -webkit-animation: fa-spin 1s infinite steps(8);
  animation: fa-spin 1s infinite steps(8);
}
@-webkit-keyframes fa-spin {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(359deg);
    transform: rotate(359deg);
  }
}
@keyframes fa-spin {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(359deg);
    transform: rotate(359deg);
  }
}
.fa-rotate-90 {
  -ms-filter: "progid:DXImageTransform.Microsoft.BasicImage(rotation=1)";
  -webkit-transform: rotate(90deg);
  -ms-transform: rotate(90deg);
  transform: rotate(90deg);
}
.fa-rotate-180 {
  -ms-filter: "progid:DXImageTransform.Microsoft.BasicImage(rotation=2)";
  -webkit-transform: rotate(180deg);
  -ms-transform: rotate(180deg);
  transform: rotate(180deg);
}
.fa-rotate-270 {
  -ms-filter: "progid:DXImageTransform.Microsoft.BasicImage(rotation=3)";
  -webkit-transform: rotate(270deg);
  -ms-transform: rotate(270deg);
  transform: rotate(270deg);
}
.fa-flip-horizontal {
  -ms-filter: "progid:DXImageTransform.Microsoft.BasicImage(rotation=0, mirror=1)";
  -webkit-transform: scale(-1, 1);
  -ms-transform: scale(-1, 1);
  transform: scale(-1, 1);
}
.fa-flip-vertical {
  -ms-filter: "progid:DXImageTransform.Microsoft.BasicImage(rotation=2, mirror=1)";
  -webkit-transform: scale(1, -1);
  -ms-transform: scale(1, -1);
  transform: scale(1, -1);
}
:root .fa-rotate-90,
:root .fa-rotate-180,
:root .fa-rotate-270,
:root .fa-flip-horizontal,
:root .fa-flip-vertical {
  filter: none;
}
.fa-stack {
  position: relative;
  display: inline-block;
  width: 2em;
  height: 2em;
  line-height: 2em;
  vertical-align: middle;
}
.fa-stack-1x,
.fa-stack-2x {
  position: absolute;
  left: 0;
  width: 100%;
  text-align: center;
}
.fa-stack-1x {
  line-height: inherit;
}
.fa-stack-2x {
  font-size: 2em;
}
.fa-inverse {
  color: #fff;
}
/* Font Awesome uses the Unicode Private Use Area (PUA) to ensure screen
   readers do not read off random characters that represent icons */
.fa-glass:before {
  content: "\f000";
}
.fa-music:before {
  content: "\f001";
}
.fa-search:before {
  content: "\f002";
}
.fa-envelope-o:before {
  content: "\f003";
}
.fa-heart:before {
  content: "\f004";
}
.fa-star:before {
  content: "\f005";
}
.fa-star-o:before {
  content: "\f006";
}
.fa-user:before {
  content: "\f007";
}
.fa-film:before {
  content: "\f008";
}
.fa-th-large:before {
  content: "\f009";
}
.fa-th:before {
  content: "\f00a";
}
.fa-th-list:before {
  content: "\f00b";
}
.fa-check:before {
  content: "\f00c";
}
.fa-remove:before,
.fa-close:before,
.fa-times:before {
  content: "\f00d";
}
.fa-search-plus:before {
  content: "\f00e";
}
.fa-search-minus:before {
  content: "\f010";
}
.fa-power-off:before {
  content: "\f011";
}
.fa-signal:before {
  content: "\f012";
}
.fa-gear:before,
.fa-cog:before {
  content: "\f013";
}
.fa-trash-o:before {
  content: "\f014";
}
.fa-home:before {
  content: "\f015";
}
.fa-file-o:before {
  content: "\f016";
}
.fa-clock-o:before {
  content: "\f017";
}
.fa-road:before {
  content: "\f018";
}
.fa-download:before {
  content: "\f019";
}
.fa-arrow-circle-o-down:before {
  content: "\f01a";
}
.fa-arrow-circle-o-up:before {
  content: "\f01b";
}
.fa-inbox:before {
  content: "\f01c";
}
.fa-play-circle-o:before {
  content: "\f01d";
}
.fa-rotate-right:before,
.fa-repeat:before {
  content: "\f01e";
}
.fa-refresh:before {
  content: "\f021";
}
.fa-list-alt:before {
  content: "\f022";
}
.fa-lock:before {
  content: "\f023";
}
.fa-flag:before {
  content: "\f024";
}
.fa-headphones:before {
  content: "\f025";
}
.fa-volume-off:before {
  content: "\f026";
}
.fa-volume-down:before {
  content: "\f027";
}
.fa-volume-up:before {
  content: "\f028";
}
.fa-qrcode:before {
  content: "\f029";
}
.fa-barcode:before {
  content: "\f02a";
}
.fa-tag:before {
  content: "\f02b";
}
.fa-tags:before {
  content: "\f02c";
}
.fa-book:before {
  content: "\f02d";
}
.fa-bookmark:before {
  content: "\f02e";
}
.fa-print:before {
  content: "\f02f";
}
.fa-camera:before {
  content: "\f030";
}
.fa-font:before {
  content: "\f031";
}
.fa-bold:before {
  content: "\f032";
}
.fa-italic:before {
  content: "\f033";
}
.fa-text-height:before {
  content: "\f034";
}
.fa-text-width:before {
  content: "\f035";
}
.fa-align-left:before {
  content: "\f036";
}
.fa-align-center:before {
  content: "\f037";
}
.fa-align-right:before {
  content: "\f038";
}
.fa-align-justify:before {
  content: "\f039";
}
.fa-list:before {
  content: "\f03a";
}
.fa-dedent:before,
.fa-outdent:before {
  content: "\f03b";
}
.fa-indent:before {
  content: "\f03c";
}
.fa-video-camera:before {
  content: "\f03d";
}
.fa-photo:before,
.fa-image:before,
.fa-picture-o:before {
  content: "\f03e";
}
.fa-pencil:before {
  content: "\f040";
}
.fa-map-marker:before {
  content: "\f041";
}
.fa-adjust:before {
  content: "\f042";
}
.fa-tint:before {
  content: "\f043";
}
.fa-edit:before,
.fa-pencil-square-o:before {
  content: "\f044";
}
.fa-share-square-o:before {
  content: "\f045";
}
.fa-check-square-o:before {
  content: "\f046";
}
.fa-arrows:before {
  content: "\f047";
}
.fa-step-backward:before {
  content: "\f048";
}
.fa-fast-backward:before {
  content: "\f049";
}
.fa-backward:before {
  content: "\f04a";
}
.fa-play:before {
  content: "\f04b";
}
.fa-pause:before {
  content: "\f04c";
}
.fa-stop:before {
  content: "\f04d";
}
.fa-forward:before {
  content: "\f04e";
}
.fa-fast-forward:before {
  content: "\f050";
}
.fa-step-forward:before {
  content: "\f051";
}
.fa-eject:before {
  content: "\f052";
}
.fa-chevron-left:before {
  content: "\f053";
}
.fa-chevron-right:before {
  content: "\f054";
}
.fa-plus-circle:before {
  content: "\f055";
}
.fa-minus-circle:before {
  content: "\f056";
}
.fa-times-circle:before {
  content: "\f057";
}
.fa-check-circle:before {
  content: "\f058";
}
.fa-question-circle:before {
  content: "\f059";
}
.fa-info-circle:before {
  content: "\f05a";
}
.fa-crosshairs:before {
  content: "\f05b";
}
.fa-times-circle-o:before {
  content: "\f05c";
}
.fa-check-circle-o:before {
  content: "\f05d";
}
.fa-ban:before {
  content: "\f05e";
}
.fa-arrow-left:before {
  content: "\f060";
}
.fa-arrow-right:before {
  content: "\f061";
}
.fa-arrow-up:before {
  content: "\f062";
}
.fa-arrow-down:before {
  content: "\f063";
}
.fa-mail-forward:before,
.fa-share:before {
  content: "\f064";
}
.fa-expand:before {
  content: "\f065";
}
.fa-compress:before {
  content: "\f066";
}
.fa-plus:before {
  content: "\f067";
}
.fa-minus:before {
  content: "\f068";
}
.fa-asterisk:before {
  content: "\f069";
}
.fa-exclamation-circle:before {
  content: "\f06a";
}
.fa-gift:before {
  content: "\f06b";
}
.fa-leaf:before {
  content: "\f06c";
}
.fa-fire:before {
  content: "\f06d";
}
.fa-eye:before {
  content: "\f06e";
}
.fa-eye-slash:before {
  content: "\f070";
}
.fa-warning:before,
.fa-exclamation-triangle:before {
  content: "\f071";
}
.fa-plane:before {
  content: "\f072";
}
.fa-calendar:before {
  content: "\f073";
}
.fa-random:before {
  content: "\f074";
}
.fa-comment:before {
  content: "\f075";
}
.fa-magnet:before {
  content: "\f076";
}
.fa-chevron-up:before {
  content: "\f077";
}
.fa-chevron-down:before {
  content: "\f078";
}
.fa-retweet:before {
  content: "\f079";
}
.fa-shopping-cart:before {
  content: "\f07a";
}
.fa-folder:before {
  content: "\f07b";
}
.fa-folder-open:before {
  content: "\f07c";
}
.fa-arrows-v:before {
  content: "\f07d";
}
.fa-arrows-h:before {
  content: "\f07e";
}
.fa-bar-chart-o:before,
.fa-bar-chart:before {
  content: "\f080";
}
.fa-twitter-square:before {
  content: "\f081";
}
.fa-facebook-square:before {
  content: "\f082";
}
.fa-camera-retro:before {
  content: "\f083";
}
.fa-key:before {
  content: "\f084";
}
.fa-gears:before,
.fa-cogs:before {
  content: "\f085";
}
.fa-comments:before {
  content: "\f086";
}
.fa-thumbs-o-up:before {
  content: "\f087";
}
.fa-thumbs-o-down:before {
  content: "\f088";
}
.fa-star-half:before {
  content: "\f089";
}
.fa-heart-o:before {
  content: "\f08a";
}
.fa-sign-out:before {
  content: "\f08b";
}
.fa-linkedin-square:before {
  content: "\f08c";
}
.fa-thumb-tack:before {
  content: "\f08d";
}
.fa-external-link:before {
  content: "\f08e";
}
.fa-sign-in:before {
  content: "\f090";
}
.fa-trophy:before {
  content: "\f091";
}
.fa-github-square:before {
  content: "\f092";
}
.fa-upload:before {
  content: "\f093";
}
.fa-lemon-o:before {
  content: "\f094";
}
.fa-phone:before {
  content: "\f095";
}
.fa-square-o:before {
  content: "\f096";
}
.fa-bookmark-o:before {
  content: "\f097";
}
.fa-phone-square:before {
  content: "\f098";
}
.fa-twitter:before {
  content: "\f099";
}
.fa-facebook-f:before,
.fa-facebook:before {
  content: "\f09a";
}
.fa-github:before {
  content: "\f09b";
}
.fa-unlock:before {
  content: "\f09c";
}
.fa-credit-card:before {
  content: "\f09d";
}
.fa-feed:before,
.fa-rss:before {
  content: "\f09e";
}
.fa-hdd-o:before {
  content: "\f0a0";
}
.fa-bullhorn:before {
  content: "\f0a1";
}
.fa-bell:before {
  content: "\f0f3";
}
.fa-certificate:before {
  content: "\f0a3";
}
.fa-hand-o-right:before {
  content: "\f0a4";
}
.fa-hand-o-left:before {
  content: "\f0a5";
}
.fa-hand-o-up:before {
  content: "\f0a6";
}
.fa-hand-o-down:before {
  content: "\f0a7";
}
.fa-arrow-circle-left:before {
  content: "\f0a8";
}
.fa-arrow-circle-right:before {
  content: "\f0a9";
}
.fa-arrow-circle-up:before {
  content: "\f0aa";
}
.fa-arrow-circle-down:before {
  content: "\f0ab";
}
.fa-globe:before {
  content: "\f0ac";
}
.fa-wrench:before {
  content: "\f0ad";
}
.fa-tasks:before {
  content: "\f0ae";
}
.fa-filter:before {
  content: "\f0b0";
}
.fa-briefcase:before {
  content: "\f0b1";
}
.fa-arrows-alt:before {
  content: "\f0b2";
}
.fa-group:before,
.fa-users:before {
  content: "\f0c0";
}
.fa-chain:before,
.fa-link:before {
  content: "\f0c1";
}
.fa-cloud:before {
  content: "\f0c2";
}
.fa-flask:before {
  content: "\f0c3";
}
.fa-cut:before,
.fa-scissors:before {
  content: "\f0c4";
}
.fa-copy:before,
.fa-files-o:before {
  content: "\f0c5";
}
.fa-paperclip:before {
  content: "\f0c6";
}
.fa-save:before,
.fa-floppy-o:before {
  content: "\f0c7";
}
.fa-square:before {
  content: "\f0c8";
}
.fa-navicon:before,
.fa-reorder:before,
.fa-bars:before {
  content: "\f0c9";
}
.fa-list-ul:before {
  content: "\f0ca";
}
.fa-list-ol:before {
  content: "\f0cb";
}
.fa-strikethrough:before {
  content: "\f0cc";
}
.fa-underline:before {
  content: "\f0cd";
}
.fa-table:before {
  content: "\f0ce";
}
.fa-magic:before {
  content: "\f0d0";
}
.fa-truck:before {
  content: "\f0d1";
}
.fa-pinterest:before {
  content: "\f0d2";
}
.fa-pinterest-square:before {
  content: "\f0d3";
}
.fa-google-plus-square:before {
  content: "\f0d4";
}
.fa-google-plus:before {
  content: "\f0d5";
}
.fa-money:before {
  content: "\f0d6";
}
.fa-caret-down:before {
  content: "\f0d7";
}
.fa-caret-up:before {
  content: "\f0d8";
}
.fa-caret-left:before {
  content: "\f0d9";
}
.fa-caret-right:before {
  content: "\f0da";
}
.fa-columns:before {
  content: "\f0db";
}
.fa-unsorted:before,
.fa-sort:before {
  content: "\f0dc";
}
.fa-sort-down:before,
.fa-sort-desc:before {
  content: "\f0dd";
}
.fa-sort-up:before,
.fa-sort-asc:before {
  content: "\f0de";
}
.fa-envelope:before {
  content: "\f0e0";
}
.fa-linkedin:before {
  content: "\f0e1";
}
.fa-rotate-left:before,
.fa-undo:before {
  content: "\f0e2";
}
.fa-legal:before,
.fa-gavel:before {
  content: "\f0e3";
}
.fa-dashboard:before,
.fa-tachometer:before {
  content: "\f0e4";
}
.fa-comment-o:before {
  content: "\f0e5";
}
.fa-comments-o:before {
  content: "\f0e6";
}
.fa-flash:before,
.fa-bolt:before {
  content: "\f0e7";
}
.fa-sitemap:before {
  content: "\f0e8";
}
.fa-umbrella:before {
  content: "\f0e9";
}
.fa-paste:before,
.fa-clipboard:before {
  content: "\f0ea";
}
.fa-lightbulb-o:before {
  content: "\f0eb";
}
.fa-exchange:before {
  content: "\f0ec";
}
.fa-cloud-download:before {
  content: "\f0ed";
}
.fa-cloud-upload:before {
  content: "\f0ee";
}
.fa-user-md:before {
  content: "\f0f0";
}
.fa-stethoscope:before {
  content: "\f0f1";
}
.fa-suitcase:before {
  content: "\f0f2";
}
.fa-bell-o:before {
  content: "\f0a2";
}
.fa-coffee:before {
  content: "\f0f4";
}
.fa-cutlery:before {
  content: "\f0f5";
}
.fa-file-text-o:before {
  content: "\f0f6";
}
.fa-building-o:before {
  content: "\f0f7";
}
.fa-hospital-o:before {
  content: "\f0f8";
}
.fa-ambulance:before {
  content: "\f0f9";
}
.fa-medkit:before {
  content: "\f0fa";
}
.fa-fighter-jet:before {
  content: "\f0fb";
}
.fa-beer:before {
  content: "\f0fc";
}
.fa-h-square:before {
  content: "\f0fd";
}
.fa-plus-square:before {
  content: "\f0fe";
}
.fa-angle-double-left:before {
  content: "\f100";
}
.fa-angle-double-right:before {
  content: "\f101";
}
.fa-angle-double-up:before {
  content: "\f102";
}
.fa-angle-double-down:before {
  content: "\f103";
}
.fa-angle-left:before {
  content: "\f104";
}
.fa-angle-right:before {
  content: "\f105";
}
.fa-angle-up:before {
  content: "\f106";
}
.fa-angle-down:before {
  content: "\f107";
}
.fa-desktop:before {
  content: "\f108";
}
.fa-laptop:before {
  content: "\f109";
}
.fa-tablet:before {
  content: "\f10a";
}
.fa-mobile-phone:before,
.fa-mobile:before {
  content: "\f10b";
}
.fa-circle-o:before {
  content: "\f10c";
}
.fa-quote-left:before {
  content: "\f10d";
}
.fa-quote-right:before {
  content: "\f10e";
}
.fa-spinner:before {
  content: "\f110";
}
.fa-circle:before {
  content: "\f111";
}
.fa-mail-reply:before,
.fa-reply:before {
  content: "\f112";
}
.fa-github-alt:before {
  content: "\f113";
}
.fa-folder-o:before {
  content: "\f114";
}
.fa-folder-open-o:before {
  content: "\f115";
}
.fa-smile-o:before {
  content: "\f118";
}
.fa-frown-o:before {
  content: "\f119";
}
.fa-meh-o:before {
  content: "\f11a";
}
.fa-gamepad:before {
  content: "\f11b";
}
.fa-keyboard-o:before {
  content: "\f11c";
}
.fa-flag-o:before {
  content: "\f11d";
}
.fa-flag-checkered:before {
  content: "\f11e";
}
.fa-terminal:before {
  content: "\f120";
}
.fa-code:before {
  content: "\f121";
}
.fa-mail-reply-all:before,
.fa-reply-all:before {
  content: "\f122";
}
.fa-star-half-empty:before,
.fa-star-half-full:before,
.fa-star-half-o:before {
  content: "\f123";
}
.fa-location-arrow:before {
  content: "\f124";
}
.fa-crop:before {
  content: "\f125";
}
.fa-code-fork:before {
  content: "\f126";
}
.fa-unlink:before,
.fa-chain-broken:before {
  content: "\f127";
}
.fa-question:before {
  content: "\f128";
}
.fa-info:before {
  content: "\f129";
}
.fa-exclamation:before {
  content: "\f12a";
}
.fa-superscript:before {
  content: "\f12b";
}
.fa-subscript:before {
  content: "\f12c";
}
.fa-eraser:before {
  content: "\f12d";
}
.fa-puzzle-piece:before {
  content: "\f12e";
}
.fa-microphone:before {
  content: "\f130";
}
.fa-microphone-slash:before {
  content: "\f131";
}
.fa-shield:before {
  content: "\f132";
}
.fa-calendar-o:before {
  content: "\f133";
}
.fa-fire-extinguisher:before {
  content: "\f134";
}
.fa-rocket:before {
  content: "\f135";
}
.fa-maxcdn:before {
  content: "\f136";
}
.fa-chevron-circle-left:before {
  content: "\f137";
}
.fa-chevron-circle-right:before {
  content: "\f138";
}
.fa-chevron-circle-up:before {
  content: "\f139";
}
.fa-chevron-circle-down:before {
  content: "\f13a";
}
.fa-html5:before {
  content: "\f13b";
}
.fa-css3:before {
  content: "\f13c";
}
.fa-anchor:before {
  content: "\f13d";
}
.fa-unlock-alt:before {
  content: "\f13e";
}
.fa-bullseye:before {
  content: "\f140";
}
.fa-ellipsis-h:before {
  content: "\f141";
}
.fa-ellipsis-v:before {
  content: "\f142";
}
.fa-rss-square:before {
  content: "\f143";
}
.fa-play-circle:before {
  content: "\f144";
}
.fa-ticket:before {
  content: "\f145";
}
.fa-minus-square:before {
  content: "\f146";
}
.fa-minus-square-o:before {
  content: "\f147";
}
.fa-level-up:before {
  content: "\f148";
}
.fa-level-down:before {
  content: "\f149";
}
.fa-check-square:before {
  content: "\f14a";
}
.fa-pencil-square:before {
  content: "\f14b";
}
.fa-external-link-square:before {
  content: "\f14c";
}
.fa-share-square:before {
  content: "\f14d";
}
.fa-compass:before {
  content: "\f14e";
}
.fa-toggle-down:before,
.fa-caret-square-o-down:before {
  content: "\f150";
}
.fa-toggle-up:before,
.fa-caret-square-o-up:before {
  content: "\f151";
}
.fa-toggle-right:before,
.fa-caret-square-o-right:before {
  content: "\f152";
}
.fa-euro:before,
.fa-eur:before {
  content: "\f153";
}
.fa-gbp:before {
  content: "\f154";
}
.fa-dollar:before,
.fa-usd:before {
  content: "\f155";
}
.fa-rupee:before,
.fa-inr:before {
  content: "\f156";
}
.fa-cny:before,
.fa-rmb:before,
.fa-yen:before,
.fa-jpy:before {
  content: "\f157";
}
.fa-ruble:before,
.fa-rouble:before,
.fa-rub:before {
  content: "\f158";
}
.fa-won:before,
.fa-krw:before {
  content: "\f159";
}
.fa-bitcoin:before,
.fa-btc:before {
  content: "\f15a";
}
.fa-file:before {
  content: "\f15b";
}
.fa-file-text:before {
  content: "\f15c";
}
.fa-sort-alpha-asc:before {
  content: "\f15d";
}
.fa-sort-alpha-desc:before {
  content: "\f15e";
}
.fa-sort-amount-asc:before {
  content: "\f160";
}
.fa-sort-amount-desc:before {
  content: "\f161";
}
.fa-sort-numeric-asc:before {
  content: "\f162";
}
.fa-sort-numeric-desc:before {
  content: "\f163";
}
.fa-thumbs-up:before {
  content: "\f164";
}
.fa-thumbs-down:before {
  content: "\f165";
}
.fa-youtube-square:before {
  content: "\f166";
}
.fa-youtube:before {
  content: "\f167";
}
.fa-xing:before {
  content: "\f168";
}
.fa-xing-square:before {
  content: "\f169";
}
.fa-youtube-play:before {
  content: "\f16a";
}
.fa-dropbox:before {
  content: "\f16b";
}
.fa-stack-overflow:before {
  content: "\f16c";
}
.fa-instagram:before {
  content: "\f16d";
}
.fa-flickr:before {
  content: "\f16e";
}
.fa-adn:before {
  content: "\f170";
}
.fa-bitbucket:before {
  content: "\f171";
}
.fa-bitbucket-square:before {
  content: "\f172";
}
.fa-tumblr:before {
  content: "\f173";
}
.fa-tumblr-square:before {
  content: "\f174";
}
.fa-long-arrow-down:before {
  content: "\f175";
}
.fa-long-arrow-up:before {
  content: "\f176";
}
.fa-long-arrow-left:before {
  content: "\f177";
}
.fa-long-arrow-right:before {
  content: "\f178";
}
.fa-apple:before {
  content: "\f179";
}
.fa-windows:before {
  content: "\f17a";
}
.fa-android:before {
  content: "\f17b";
}
.fa-linux:before {
  content: "\f17c";
}
.fa-dribbble:before {
  content: "\f17d";
}
.fa-skype:before {
  content: "\f17e";
}
.fa-foursquare:before {
  content: "\f180";
}
.fa-trello:before {
  content: "\f181";
}
.fa-female:before {
  content: "\f182";
}
.fa-male:before {
  content: "\f183";
}
.fa-gittip:before,
.fa-gratipay:before {
  content: "\f184";
}
.fa-sun-o:before {
  content: "\f185";
}
.fa-moon-o:before {
  content: "\f186";
}
.fa-archive:before {
  content: "\f187";
}
.fa-bug:before {
  content: "\f188";
}
.fa-vk:before {
  content: "\f189";
}
.fa-weibo:before {
  content: "\f18a";
}
.fa-renren:before {
  content: "\f18b";
}
.fa-pagelines:before {
  content: "\f18c";
}
.fa-stack-exchange:before {
  content: "\f18d";
}
.fa-arrow-circle-o-right:before {
  content: "\f18e";
}
.fa-arrow-circle-o-left:before {
  content: "\f190";
}
.fa-toggle-left:before,
.fa-caret-square-o-left:before {
  content: "\f191";
}
.fa-dot-circle-o:before {
  content: "\f192";
}
.fa-wheelchair:before {
  content: "\f193";
}
.fa-vimeo-square:before {
  content: "\f194";
}
.fa-turkish-lira:before,
.fa-try:before {
  content: "\f195";
}
.fa-plus-square-o:before {
  content: "\f196";
}
.fa-space-shuttle:before {
  content: "\f197";
}
.fa-slack:before {
  content: "\f198";
}
.fa-envelope-square:before {
  content: "\f199";
}
.fa-wordpress:before {
  content: "\f19a";
}
.fa-openid:before {
  content: "\f19b";
}
.fa-institution:before,
.fa-bank:before,
.fa-university:before {
  content: "\f19c";
}
.fa-mortar-board:before,
.fa-graduation-cap:before {
  content: "\f19d";
}
.fa-yahoo:before {
  content: "\f19e";
}
.fa-google:before {
  content: "\f1a0";
}
.fa-reddit:before {
  content: "\f1a1";
}
.fa-reddit-square:before {
  content: "\f1a2";
}
.fa-stumbleupon-circle:before {
  content: "\f1a3";
}
.fa-stumbleupon:before {
  content: "\f1a4";
}
.fa-delicious:before {
  content: "\f1a5";
}
.fa-digg:before {
  content: "\f1a6";
}
.fa-pied-piper-pp:before {
  content: "\f1a7";
}
.fa-pied-piper-alt:before {
  content: "\f1a8";
}
.fa-drupal:before {
  content: "\f1a9";
}
.fa-joomla:before {
  content: "\f1aa";
}
.fa-language:before {
  content: "\f1ab";
}
.fa-fax:before {
  content: "\f1ac";
}
.fa-building:before {
  content: "\f1ad";
}
.fa-child:before {
  content: "\f1ae";
}
.fa-paw:before {
  content: "\f1b0";
}
.fa-spoon:before {
  content: "\f1b1";
}
.fa-cube:before {
  content: "\f1b2";
}
.fa-cubes:before {
  content: "\f1b3";
}
.fa-behance:before {
  content: "\f1b4";
}
.fa-behance-square:before {
  content: "\f1b5";
}
.fa-steam:before {
  content: "\f1b6";
}
.fa-steam-square:before {
  content: "\f1b7";
}
.fa-recycle:before {
  content: "\f1b8";
}
.fa-automobile:before,
.fa-car:before {
  content: "\f1b9";
}
.fa-cab:before,
.fa-taxi:before {
  content: "\f1ba";
}
.fa-tree:before {
  content: "\f1bb";
}
.fa-spotify:before {
  content: "\f1bc";
}
.fa-deviantart:before {
  content: "\f1bd";
}
.fa-soundcloud:before {
  content: "\f1be";
}
.fa-database:before {
  content: "\f1c0";
}
.fa-file-pdf-o:before {
  content: "\f1c1";
}
.fa-file-word-o:before {
  content: "\f1c2";
}
.fa-file-excel-o:before {
  content: "\f1c3";
}
.fa-file-powerpoint-o:before {
  content: "\f1c4";
}
.fa-file-photo-o:before,
.fa-file-picture-o:before,
.fa-file-image-o:before {
  content: "\f1c5";
}
.fa-file-zip-o:before,
.fa-file-archive-o:before {
  content: "\f1c6";
}
.fa-file-sound-o:before,
.fa-file-audio-o:before {
  content: "\f1c7";
}
.fa-file-movie-o:before,
.fa-file-video-o:before {
  content: "\f1c8";
}
.fa-file-code-o:before {
  content: "\f1c9";
}
.fa-vine:before {
  content: "\f1ca";
}
.fa-codepen:before {
  content: "\f1cb";
}
.fa-jsfiddle:before {
  content: "\f1cc";
}
.fa-life-bouy:before,
.fa-life-buoy:before,
.fa-life-saver:before,
.fa-support:before,
.fa-life-ring:before {
  content: "\f1cd";
}
.fa-circle-o-notch:before {
  content: "\f1ce";
}
.fa-ra:before,
.fa-resistance:before,
.fa-rebel:before {
  content: "\f1d0";
}
.fa-ge:before,
.fa-empire:before {
  content: "\f1d1";
}
.fa-git-square:before {
  content: "\f1d2";
}
.fa-git:before {
  content: "\f1d3";
}
.fa-y-combinator-square:before,
.fa-yc-square:before,
.fa-hacker-news:before {
  content: "\f1d4";
}
.fa-tencent-weibo:before {
  content: "\f1d5";
}
.fa-qq:before {
  content: "\f1d6";
}
.fa-wechat:before,
.fa-weixin:before {
  content: "\f1d7";
}
.fa-send:before,
.fa-paper-plane:before {
  content: "\f1d8";
}
.fa-send-o:before,
.fa-paper-plane-o:before {
  content: "\f1d9";
}
.fa-history:before {
  content: "\f1da";
}
.fa-circle-thin:before {
  content: "\f1db";
}
.fa-header:before {
  content: "\f1dc";
}
.fa-paragraph:before {
  content: "\f1dd";
}
.fa-sliders:before {
  content: "\f1de";
}
.fa-share-alt:before {
  content: "\f1e0";
}
.fa-share-alt-square:before {
  content: "\f1e1";
}
.fa-bomb:before {
  content: "\f1e2";
}
.fa-soccer-ball-o:before,
.fa-futbol-o:before {
  content: "\f1e3";
}
.fa-tty:before {
  content: "\f1e4";
}
.fa-binoculars:before {
  content: "\f1e5";
}
.fa-plug:before {
  content: "\f1e6";
}
.fa-slideshare:before {
  content: "\f1e7";
}
.fa-twitch:before {
  content: "\f1e8";
}
.fa-yelp:before {
  content: "\f1e9";
}
.fa-newspaper-o:before {
  content: "\f1ea";
}
.fa-wifi:before {
  content: "\f1eb";
}
.fa-calculator:before {
  content: "\f1ec";
}
.fa-paypal:before {
  content: "\f1ed";
}
.fa-google-wallet:before {
  content: "\f1ee";
}
.fa-cc-visa:before {
  content: "\f1f0";
}
.fa-cc-mastercard:before {
  content: "\f1f1";
}
.fa-cc-discover:before {
  content: "\f1f2";
}
.fa-cc-amex:before {
  content: "\f1f3";
}
.fa-cc-paypal:before {
  content: "\f1f4";
}
.fa-cc-stripe:before {
  content: "\f1f5";
}
.fa-bell-slash:before {
  content: "\f1f6";
}
.fa-bell-slash-o:before {
  content: "\f1f7";
}
.fa-trash:before {
  content: "\f1f8";
}
.fa-copyright:before {
  content: "\f1f9";
}
.fa-at:before {
  content: "\f1fa";
}
.fa-eyedropper:before {
  content: "\f1fb";
}
.fa-paint-brush:before {
  content: "\f1fc";
}
.fa-birthday-cake:before {
  content: "\f1fd";
}
.fa-area-chart:before {
  content: "\f1fe";
}
.fa-pie-chart:before {
  content: "\f200";
}
.fa-line-chart:before {
  content: "\f201";
}
.fa-lastfm:before {
  content: "\f202";
}
.fa-lastfm-square:before {
  content: "\f203";
}
.fa-toggle-off:before {
  content: "\f204";
}
.fa-toggle-on:before {
  content: "\f205";
}
.fa-bicycle:before {
  content: "\f206";
}
.fa-bus:before {
  content: "\f207";
}
.fa-ioxhost:before {
  content: "\f208";
}
.fa-angellist:before {
  content: "\f209";
}
.fa-cc:before {
  content: "\f20a";
}
.fa-shekel:before,
.fa-sheqel:before,
.fa-ils:before {
  content: "\f20b";
}
.fa-meanpath:before {
  content: "\f20c";
}
.fa-buysellads:before {
  content: "\f20d";
}
.fa-connectdevelop:before {
  content: "\f20e";
}
.fa-dashcube:before {
  content: "\f210";
}
.fa-forumbee:before {
  content: "\f211";
}
.fa-leanpub:before {
  content: "\f212";
}
.fa-sellsy:before {
  content: "\f213";
}
.fa-shirtsinbulk:before {
  content: "\f214";
}
.fa-simplybuilt:before {
  content: "\f215";
}
.fa-skyatlas:before {
  content: "\f216";
}
.fa-cart-plus:before {
  content: "\f217";
}
.fa-cart-arrow-down:before {
  content: "\f218";
}
.fa-diamond:before {
  content: "\f219";
}
.fa-ship:before {
  content: "\f21a";
}
.fa-user-secret:before {
  content: "\f21b";
}
.fa-motorcycle:before {
  content: "\f21c";
}
.fa-street-view:before {
  content: "\f21d";
}
.fa-heartbeat:before {
  content: "\f21e";
}
.fa-venus:before {
  content: "\f221";
}
.fa-mars:before {
  content: "\f222";
}
.fa-mercury:before {
  content: "\f223";
}
.fa-intersex:before,
.fa-transgender:before {
  content: "\f224";
}
.fa-transgender-alt:before {
  content: "\f225";
}
.fa-venus-double:before {
  content: "\f226";
}
.fa-mars-double:before {
  content: "\f227";
}
.fa-venus-mars:before {
  content: "\f228";
}
.fa-mars-stroke:before {
  content: "\f229";
}
.fa-mars-stroke-v:before {
  content: "\f22a";
}
.fa-mars-stroke-h:before {
  content: "\f22b";
}
.fa-neuter:before {
  content: "\f22c";
}
.fa-genderless:before {
  content: "\f22d";
}
.fa-facebook-official:before {
  content: "\f230";
}
.fa-pinterest-p:before {
  content: "\f231";
}
.fa-whatsapp:before {
  content: "\f232";
}
.fa-server:before {
  content: "\f233";
}
.fa-user-plus:before {
  content: "\f234";
}
.fa-user-times:before {
  content: "\f235";
}
.fa-hotel:before,
.fa-bed:before {
  content: "\f236";
}
.fa-viacoin:before {
  content: "\f237";
}
.fa-train:before {
  content: "\f238";
}
.fa-subway:before {
  content: "\f239";
}
.fa-medium:before {
  content: "\f23a";
}
.fa-yc:before,
.fa-y-combinator:before {
  content: "\f23b";
}
.fa-optin-monster:before {
  content: "\f23c";
}
.fa-opencart:before {
  content: "\f23d";
}
.fa-expeditedssl:before {
  content: "\f23e";
}
.fa-battery-4:before,
.fa-battery:before,
.fa-battery-full:before {
  content: "\f240";
}
.fa-battery-3:before,
.fa-battery-three-quarters:before {
  content: "\f241";
}
.fa-battery-2:before,
.fa-battery-half:before {
  content: "\f242";
}
.fa-battery-1:before,
.fa-battery-quarter:before {
  content: "\f243";
}
.fa-battery-0:before,
.fa-battery-empty:before {
  content: "\f244";
}
.fa-mouse-pointer:before {
  content: "\f245";
}
.fa-i-cursor:before {
  content: "\f246";
}
.fa-object-group:before {
  content: "\f247";
}
.fa-object-ungroup:before {
  content: "\f248";
}
.fa-sticky-note:before {
  content: "\f249";
}
.fa-sticky-note-o:before {
  content: "\f24a";
}
.fa-cc-jcb:before {
  content: "\f24b";
}
.fa-cc-diners-club:before {
  content: "\f24c";
}
.fa-clone:before {
  content: "\f24d";
}
.fa-balance-scale:before {
  content: "\f24e";
}
.fa-hourglass-o:before {
  content: "\f250";
}
.fa-hourglass-1:before,
.fa-hourglass-start:before {
  content: "\f251";
}
.fa-hourglass-2:before,
.fa-hourglass-half:before {
  content: "\f252";
}
.fa-hourglass-3:before,
.fa-hourglass-end:before {
  content: "\f253";
}
.fa-hourglass:before {
  content: "\f254";
}
.fa-hand-grab-o:before,
.fa-hand-rock-o:before {
  content: "\f255";
}
.fa-hand-stop-o:before,
.fa-hand-paper-o:before {
  content: "\f256";
}
.fa-hand-scissors-o:before {
  content: "\f257";
}
.fa-hand-lizard-o:before {
  content: "\f258";
}
.fa-hand-spock-o:before {
  content: "\f259";
}
.fa-hand-pointer-o:before {
  content: "\f25a";
}
.fa-hand-peace-o:before {
  content: "\f25b";
}
.fa-trademark:before {
  content: "\f25c";
}
.fa-registered:before {
  content: "\f25d";
}
.fa-creative-commons:before {
  content: "\f25e";
}
.fa-gg:before {
  content: "\f260";
}
.fa-gg-circle:before {
  content: "\f261";
}
.fa-tripadvisor:before {
  content: "\f262";
}
.fa-odnoklassniki:before {
  content: "\f263";
}
.fa-odnoklassniki-square:before {
  content: "\f264";
}
.fa-get-pocket:before {
  content: "\f265";
}
.fa-wikipedia-w:before {
  content: "\f266";
}
.fa-safari:before {
  content: "\f267";
}
.fa-chrome:before {
  content: "\f268";
}
.fa-firefox:before {
  content: "\f269";
}
.fa-opera:before {
  content: "\f26a";
}
.fa-internet-explorer:before {
  content: "\f26b";
}
.fa-tv:before,
.fa-television:before {
  content: "\f26c";
}
.fa-contao:before {
  content: "\f26d";
}
.fa-500px:before {
  content: "\f26e";
}
.fa-amazon:before {
  content: "\f270";
}
.fa-calendar-plus-o:before {
  content: "\f271";
}
.fa-calendar-minus-o:before {
  content: "\f272";
}
.fa-calendar-times-o:before {
  content: "\f273";
}
.fa-calendar-check-o:before {
  content: "\f274";
}
.fa-industry:before {
  content: "\f275";
}
.fa-map-pin:before {
  content: "\f276";
}
.fa-map-signs:before {
  content: "\f277";
}
.fa-map-o:before {
  content: "\f278";
}
.fa-map:before {
  content: "\f279";
}
.fa-commenting:before {
  content: "\f27a";
}
.fa-commenting-o:before {
  content: "\f27b";
}
.fa-houzz:before {
  content: "\f27c";
}
.fa-vimeo:before {
  content: "\f27d";
}
.fa-black-tie:before {
  content: "\f27e";
}
.fa-fonticons:before {
  content: "\f280";
}
.fa-reddit-alien:before {
  content: "\f281";
}
.fa-edge:before {
  content: "\f282";
}
.fa-credit-card-alt:before {
  content: "\f283";
}
.fa-codiepie:before {
  content: "\f284";
}
.fa-modx:before {
  content: "\f285";
}
.fa-fort-awesome:before {
  content: "\f286";
}
.fa-usb:before {
  content: "\f287";
}
.fa-product-hunt:before {
  content: "\f288";
}
.fa-mixcloud:before {
  content: "\f289";
}
.fa-scribd:before {
  content: "\f28a";
}
.fa-pause-circle:before {
  content: "\f28b";
}
.fa-pause-circle-o:before {
  content: "\f28c";
}
.fa-stop-circle:before {
  content: "\f28d";
}
.fa-stop-circle-o:before {
  content: "\f28e";
}
.fa-shopping-bag:before {
  content: "\f290";
}
.fa-shopping-basket:before {
  content: "\f291";
}
.fa-hashtag:before {
  content: "\f292";
}
.fa-bluetooth:before {
  content: "\f293";
}
.fa-bluetooth-b:before {
  content: "\f294";
}
.fa-percent:before {
  content: "\f295";
}
.fa-gitlab:before {
  content: "\f296";
}
.fa-wpbeginner:before {
  content: "\f297";
}
.fa-wpforms:before {
  content: "\f298";
}
.fa-envira:before {
  content: "\f299";
}
.fa-universal-access:before {
  content: "\f29a";
}
.fa-wheelchair-alt:before {
  content: "\f29b";
}
.fa-question-circle-o:before {
  content: "\f29c";
}
.fa-blind:before {
  content: "\f29d";
}
.fa-audio-description:before {
  content: "\f29e";
}
.fa-volume-control-phone:before {
  content: "\f2a0";
}
.fa-braille:before {
  content: "\f2a1";
}
.fa-assistive-listening-systems:before {
  content: "\f2a2";
}
.fa-asl-interpreting:before,
.fa-american-sign-language-interpreting:before {
  content: "\f2a3";
}
.fa-deafness:before,
.fa-hard-of-hearing:before,
.fa-deaf:before {
  content: "\f2a4";
}
.fa-glide:before {
  content: "\f2a5";
}
.fa-glide-g:before {
  content: "\f2a6";
}
.fa-signing:before,
.fa-sign-language:before {
  content: "\f2a7";
}
.fa-low-vision:before {
  content: "\f2a8";
}
.fa-viadeo:before {
  content: "\f2a9";
}
.fa-viadeo-square:before {
  content: "\f2aa";
}
.fa-snapchat:before {
  content: "\f2ab";
}
.fa-snapchat-ghost:before {
  content: "\f2ac";
}
.fa-snapchat-square:before {
  content: "\f2ad";
}
.fa-pied-piper:before {
  content: "\f2ae";
}
.fa-first-order:before {
  content: "\f2b0";
}
.fa-yoast:before {
  content: "\f2b1";
}
.fa-themeisle:before {
  content: "\f2b2";
}
.fa-google-plus-circle:before,
.fa-google-plus-official:before {
  content: "\f2b3";
}
.fa-fa:before,
.fa-font-awesome:before {
  content: "\f2b4";
}
.fa-handshake-o:before {
  content: "\f2b5";
}
.fa-envelope-open:before {
  content: "\f2b6";
}
.fa-envelope-open-o:before {
  content: "\f2b7";
}
.fa-linode:before {
  content: "\f2b8";
}
.fa-address-book:before {
  content: "\f2b9";
}
.fa-address-book-o:before {
  content: "\f2ba";
}
.fa-vcard:before,
.fa-address-card:before {
  content: "\f2bb";
}
.fa-vcard-o:before,
.fa-address-card-o:before {
  content: "\f2bc";
}
.fa-user-circle:before {
  content: "\f2bd";
}
.fa-user-circle-o:before {
  content: "\f2be";
}
.fa-user-o:before {
  content: "\f2c0";
}
.fa-id-badge:before {
  content: "\f2c1";
}
.fa-drivers-license:before,
.fa-id-card:before {
  content: "\f2c2";
}
.fa-drivers-license-o:before,
.fa-id-card-o:before {
  content: "\f2c3";
}
.fa-quora:before {
  content: "\f2c4";
}
.fa-free-code-camp:before {
  content: "\f2c5";
}
.fa-telegram:before {
  content: "\f2c6";
}
.fa-thermometer-4:before,
.fa-thermometer:before,
.fa-thermometer-full:before {
  content: "\f2c7";
}
.fa-thermometer-3:before,
.fa-thermometer-three-quarters:before {
  content: "\f2c8";
}
.fa-thermometer-2:before,
.fa-thermometer-half:before {
  content: "\f2c9";
}
.fa-thermometer-1:before,
.fa-thermometer-quarter:before {
  content: "\f2ca";
}
.fa-thermometer-0:before,
.fa-thermometer-empty:before {
  content: "\f2cb";
}
.fa-shower:before {
  content: "\f2cc";
}
.fa-bathtub:before,
.fa-s15:before,
.fa-bath:before {
  content: "\f2cd";
}
.fa-podcast:before {
  content: "\f2ce";
}
.fa-window-maximize:before {
  content: "\f2d0";
}
.fa-window-minimize:before {
  content: "\f2d1";
}
.fa-window-restore:before {
  content: "\f2d2";
}
.fa-times-rectangle:before,
.fa-window-close:before {
  content: "\f2d3";
}
.fa-times-rectangle-o:before,
.fa-window-close-o:before {
  content: "\f2d4";
}
.fa-bandcamp:before {
  content: "\f2d5";
}
.fa-grav:before {
  content: "\f2d6";
}
.fa-etsy:before {
  content: "\f2d7";
}
.fa-imdb:before {
  content: "\f2d8";
}
.fa-ravelry:before {
  content: "\f2d9";
}
.fa-eercast:before {
  content: "\f2da";
}
.fa-microchip:before {
  content: "\f2db";
}
.fa-snowflake-o:before {
  content: "\f2dc";
}
.fa-superpowers:before {
  content: "\f2dd";
}
.fa-wpexplorer:before {
  content: "\f2de";
}
.fa-meetup:before {
  content: "\f2e0";
}
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  border: 0;
}
.sr-only-focusable:active,
.sr-only-focusable:focus {
  position: static;
  width: auto;
  height: auto;
  margin: 0;
  overflow: visible;
  clip: auto;
}
.sr-only-focusable:active,
.sr-only-focusable:focus {
  position: static;
  width: auto;
  height: auto;
  margin: 0;
  overflow: visible;
  clip: auto;
}
/*!
*
* IPython base
*
*/
.modal.fade .modal-dialog {
  -webkit-transform: translate(0, 0);
  -ms-transform: translate(0, 0);
  -o-transform: translate(0, 0);
  transform: translate(0, 0);
}
code {
  color: #000;
}
pre {
  font-size: inherit;
  line-height: inherit;
}
label {
  font-weight: normal;
}
/* Make the page background atleast 100% the height of the view port */
/* Make the page itself atleast 70% the height of the view port */
.border-box-sizing {
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
.corner-all {
  border-radius: 2px;
}
.no-padding {
  padding: 0px;
}
/* Flexible box model classes */
/* Taken from Alex Russell http://infrequently.org/2009/08/css-3-progress/ */
/* This file is a compatability layer.  It allows the usage of flexible box 
model layouts accross multiple browsers, including older browsers.  The newest,
universal implementation of the flexible box model is used when available (see
`Modern browsers` comments below).  Browsers that are known to implement this 
new spec completely include:

    Firefox 28.0+
    Chrome 29.0+
    Internet Explorer 11+ 
    Opera 17.0+

Browsers not listed, including Safari, are supported via the styling under the
`Old browsers` comments below.
*/
.hbox {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
.hbox > * {
  /* Old browsers */
  -webkit-box-flex: 0;
  -moz-box-flex: 0;
  box-flex: 0;
  /* Modern browsers */
  flex: none;
}
.vbox {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
}
.vbox > * {
  /* Old browsers */
  -webkit-box-flex: 0;
  -moz-box-flex: 0;
  box-flex: 0;
  /* Modern browsers */
  flex: none;
}
.hbox.reverse,
.vbox.reverse,
.reverse {
  /* Old browsers */
  -webkit-box-direction: reverse;
  -moz-box-direction: reverse;
  box-direction: reverse;
  /* Modern browsers */
  flex-direction: row-reverse;
}
.hbox.box-flex0,
.vbox.box-flex0,
.box-flex0 {
  /* Old browsers */
  -webkit-box-flex: 0;
  -moz-box-flex: 0;
  box-flex: 0;
  /* Modern browsers */
  flex: none;
  width: auto;
}
.hbox.box-flex1,
.vbox.box-flex1,
.box-flex1 {
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
}
.hbox.box-flex,
.vbox.box-flex,
.box-flex {
  /* Old browsers */
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
}
.hbox.box-flex2,
.vbox.box-flex2,
.box-flex2 {
  /* Old browsers */
  -webkit-box-flex: 2;
  -moz-box-flex: 2;
  box-flex: 2;
  /* Modern browsers */
  flex: 2;
}
.box-group1 {
  /*  Deprecated */
  -webkit-box-flex-group: 1;
  -moz-box-flex-group: 1;
  box-flex-group: 1;
}
.box-group2 {
  /* Deprecated */
  -webkit-box-flex-group: 2;
  -moz-box-flex-group: 2;
  box-flex-group: 2;
}
.hbox.start,
.vbox.start,
.start {
  /* Old browsers */
  -webkit-box-pack: start;
  -moz-box-pack: start;
  box-pack: start;
  /* Modern browsers */
  justify-content: flex-start;
}
.hbox.end,
.vbox.end,
.end {
  /* Old browsers */
  -webkit-box-pack: end;
  -moz-box-pack: end;
  box-pack: end;
  /* Modern browsers */
  justify-content: flex-end;
}
.hbox.center,
.vbox.center,
.center {
  /* Old browsers */
  -webkit-box-pack: center;
  -moz-box-pack: center;
  box-pack: center;
  /* Modern browsers */
  justify-content: center;
}
.hbox.baseline,
.vbox.baseline,
.baseline {
  /* Old browsers */
  -webkit-box-pack: baseline;
  -moz-box-pack: baseline;
  box-pack: baseline;
  /* Modern browsers */
  justify-content: baseline;
}
.hbox.stretch,
.vbox.stretch,
.stretch {
  /* Old browsers */
  -webkit-box-pack: stretch;
  -moz-box-pack: stretch;
  box-pack: stretch;
  /* Modern browsers */
  justify-content: stretch;
}
.hbox.align-start,
.vbox.align-start,
.align-start {
  /* Old browsers */
  -webkit-box-align: start;
  -moz-box-align: start;
  box-align: start;
  /* Modern browsers */
  align-items: flex-start;
}
.hbox.align-end,
.vbox.align-end,
.align-end {
  /* Old browsers */
  -webkit-box-align: end;
  -moz-box-align: end;
  box-align: end;
  /* Modern browsers */
  align-items: flex-end;
}
.hbox.align-center,
.vbox.align-center,
.align-center {
  /* Old browsers */
  -webkit-box-align: center;
  -moz-box-align: center;
  box-align: center;
  /* Modern browsers */
  align-items: center;
}
.hbox.align-baseline,
.vbox.align-baseline,
.align-baseline {
  /* Old browsers */
  -webkit-box-align: baseline;
  -moz-box-align: baseline;
  box-align: baseline;
  /* Modern browsers */
  align-items: baseline;
}
.hbox.align-stretch,
.vbox.align-stretch,
.align-stretch {
  /* Old browsers */
  -webkit-box-align: stretch;
  -moz-box-align: stretch;
  box-align: stretch;
  /* Modern browsers */
  align-items: stretch;
}
div.error {
  margin: 2em;
  text-align: center;
}
div.error > h1 {
  font-size: 500%;
  line-height: normal;
}
div.error > p {
  font-size: 200%;
  line-height: normal;
}
div.traceback-wrapper {
  text-align: left;
  max-width: 800px;
  margin: auto;
}
div.traceback-wrapper pre.traceback {
  max-height: 600px;
  overflow: auto;
}
/**
 * Primary styles
 *
 * Author: Jupyter Development Team
 */
body {
  background-color: #fff;
  /* This makes sure that the body covers the entire window and needs to
       be in a different element than the display: box in wrapper below */
  position: absolute;
  left: 0px;
  right: 0px;
  top: 0px;
  bottom: 0px;
  overflow: visible;
}
body > #header {
  /* Initially hidden to prevent FLOUC */
  display: none;
  background-color: #fff;
  /* Display over codemirror */
  position: relative;
  z-index: 100;
}
body > #header #header-container {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  padding: 5px;
  padding-bottom: 5px;
  padding-top: 5px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
body > #header .header-bar {
  width: 100%;
  height: 1px;
  background: #e7e7e7;
  margin-bottom: -1px;
}
@media print {
  body > #header {
    display: none !important;
  }
}
#header-spacer {
  width: 100%;
  visibility: hidden;
}
@media print {
  #header-spacer {
    display: none;
  }
}
#ipython_notebook {
  padding-left: 0px;
  padding-top: 1px;
  padding-bottom: 1px;
}
[dir="rtl"] #ipython_notebook {
  margin-right: 10px;
  margin-left: 0;
}
[dir="rtl"] #ipython_notebook.pull-left {
  float: right !important;
  float: right;
}
.flex-spacer {
  flex: 1;
}
#noscript {
  width: auto;
  padding-top: 16px;
  padding-bottom: 16px;
  text-align: center;
  font-size: 22px;
  color: red;
  font-weight: bold;
}
#ipython_notebook img {
  height: 28px;
}
#site {
  width: 100%;
  display: none;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  overflow: auto;
}
@media print {
  #site {
    height: auto !important;
  }
}
/* Smaller buttons */
.ui-button .ui-button-text {
  padding: 0.2em 0.8em;
  font-size: 77%;
}
input.ui-button {
  padding: 0.3em 0.9em;
}
span#kernel_logo_widget {
  margin: 0 10px;
}
span#login_widget {
  float: right;
}
[dir="rtl"] span#login_widget {
  float: left;
}
span#login_widget > .button,
#logout {
  color: #333;
  background-color: #fff;
  border-color: #ccc;
}
span#login_widget > .button:focus,
#logout:focus,
span#login_widget > .button.focus,
#logout.focus {
  color: #333;
  background-color: #e6e6e6;
  border-color: #8c8c8c;
}
span#login_widget > .button:hover,
#logout:hover {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
span#login_widget > .button:active,
#logout:active,
span#login_widget > .button.active,
#logout.active,
.open > .dropdown-togglespan#login_widget > .button,
.open > .dropdown-toggle#logout {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
span#login_widget > .button:active:hover,
#logout:active:hover,
span#login_widget > .button.active:hover,
#logout.active:hover,
.open > .dropdown-togglespan#login_widget > .button:hover,
.open > .dropdown-toggle#logout:hover,
span#login_widget > .button:active:focus,
#logout:active:focus,
span#login_widget > .button.active:focus,
#logout.active:focus,
.open > .dropdown-togglespan#login_widget > .button:focus,
.open > .dropdown-toggle#logout:focus,
span#login_widget > .button:active.focus,
#logout:active.focus,
span#login_widget > .button.active.focus,
#logout.active.focus,
.open > .dropdown-togglespan#login_widget > .button.focus,
.open > .dropdown-toggle#logout.focus {
  color: #333;
  background-color: #d4d4d4;
  border-color: #8c8c8c;
}
span#login_widget > .button:active,
#logout:active,
span#login_widget > .button.active,
#logout.active,
.open > .dropdown-togglespan#login_widget > .button,
.open > .dropdown-toggle#logout {
  background-image: none;
}
span#login_widget > .button.disabled:hover,
#logout.disabled:hover,
span#login_widget > .button[disabled]:hover,
#logout[disabled]:hover,
fieldset[disabled] span#login_widget > .button:hover,
fieldset[disabled] #logout:hover,
span#login_widget > .button.disabled:focus,
#logout.disabled:focus,
span#login_widget > .button[disabled]:focus,
#logout[disabled]:focus,
fieldset[disabled] span#login_widget > .button:focus,
fieldset[disabled] #logout:focus,
span#login_widget > .button.disabled.focus,
#logout.disabled.focus,
span#login_widget > .button[disabled].focus,
#logout[disabled].focus,
fieldset[disabled] span#login_widget > .button.focus,
fieldset[disabled] #logout.focus {
  background-color: #fff;
  border-color: #ccc;
}
span#login_widget > .button .badge,
#logout .badge {
  color: #fff;
  background-color: #333;
}
.nav-header {
  text-transform: none;
}
#header > span {
  margin-top: 10px;
}
.modal_stretch .modal-dialog {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
  min-height: 80vh;
}
.modal_stretch .modal-dialog .modal-body {
  max-height: calc(100vh - 200px);
  overflow: auto;
  flex: 1;
}
.modal-header {
  cursor: move;
}
@media (min-width: 768px) {
  .modal .modal-dialog {
    width: 700px;
  }
}
@media (min-width: 768px) {
  select.form-control {
    margin-left: 12px;
    margin-right: 12px;
  }
}
/*!
*
* IPython auth
*
*/
.center-nav {
  display: inline-block;
  margin-bottom: -4px;
}
[dir="rtl"] .center-nav form.pull-left {
  float: right !important;
  float: right;
}
[dir="rtl"] .center-nav .navbar-text {
  float: right;
}
[dir="rtl"] .navbar-inner {
  text-align: right;
}
[dir="rtl"] div.text-left {
  text-align: right;
}
/*!
*
* IPython tree view
*
*/
/* We need an invisible input field on top of the sentense*/
/* "Drag file onto the list ..." */
.alternate_upload {
  background-color: none;
  display: inline;
}
.alternate_upload.form {
  padding: 0;
  margin: 0;
}
.alternate_upload input.fileinput {
  position: absolute;
  display: block;
  width: 100%;
  height: 100%;
  overflow: hidden;
  cursor: pointer;
  opacity: 0;
  z-index: 2;
}
.alternate_upload .btn-xs > input.fileinput {
  margin: -1px -5px;
}
.alternate_upload .btn-upload {
  position: relative;
  height: 22px;
}
::-webkit-file-upload-button {
  cursor: pointer;
}
/**
 * Primary styles
 *
 * Author: Jupyter Development Team
 */
ul#tabs {
  margin-bottom: 4px;
}
ul#tabs a {
  padding-top: 6px;
  padding-bottom: 4px;
}
[dir="rtl"] ul#tabs.nav-tabs > li {
  float: right;
}
[dir="rtl"] ul#tabs.nav.nav-tabs {
  padding-right: 0;
}
ul.breadcrumb a:focus,
ul.breadcrumb a:hover {
  text-decoration: none;
}
ul.breadcrumb i.icon-home {
  font-size: 16px;
  margin-right: 4px;
}
ul.breadcrumb span {
  color: #5e5e5e;
}
.list_toolbar {
  padding: 4px 0 4px 0;
  vertical-align: middle;
}
.list_toolbar .tree-buttons {
  padding-top: 1px;
}
[dir="rtl"] .list_toolbar .tree-buttons .pull-right {
  float: left !important;
  float: left;
}
[dir="rtl"] .list_toolbar .col-sm-4,
[dir="rtl"] .list_toolbar .col-sm-8 {
  float: right;
}
.dynamic-buttons {
  padding-top: 3px;
  display: inline-block;
}
.list_toolbar [class*="span"] {
  min-height: 24px;
}
.list_header {
  font-weight: bold;
  background-color: #EEE;
}
.list_placeholder {
  font-weight: bold;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 7px;
  padding-right: 7px;
}
.list_container {
  margin-top: 4px;
  margin-bottom: 20px;
  border: 1px solid #ddd;
  border-radius: 2px;
}
.list_container > div {
  border-bottom: 1px solid #ddd;
}
.list_container > div:hover .list-item {
  background-color: red;
}
.list_container > div:last-child {
  border: none;
}
.list_item:hover .list_item {
  background-color: #ddd;
}
.list_item a {
  text-decoration: none;
}
.list_item:hover {
  background-color: #fafafa;
}
.list_header > div,
.list_item > div {
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 7px;
  padding-right: 7px;
  line-height: 22px;
}
.list_header > div input,
.list_item > div input {
  margin-right: 7px;
  margin-left: 14px;
  vertical-align: text-bottom;
  line-height: 22px;
  position: relative;
  top: -1px;
}
.list_header > div .item_link,
.list_item > div .item_link {
  margin-left: -1px;
  vertical-align: baseline;
  line-height: 22px;
}
[dir="rtl"] .list_item > div input {
  margin-right: 0;
}
.new-file input[type=checkbox] {
  visibility: hidden;
}
.item_name {
  line-height: 22px;
  height: 24px;
}
.item_icon {
  font-size: 14px;
  color: #5e5e5e;
  margin-right: 7px;
  margin-left: 7px;
  line-height: 22px;
  vertical-align: baseline;
}
.item_modified {
  margin-right: 7px;
  margin-left: 7px;
}
[dir="rtl"] .item_modified.pull-right {
  float: left !important;
  float: left;
}
.item_buttons {
  line-height: 1em;
  margin-left: -5px;
}
.item_buttons .btn,
.item_buttons .btn-group,
.item_buttons .input-group {
  float: left;
}
.item_buttons > .btn,
.item_buttons > .btn-group,
.item_buttons > .input-group {
  margin-left: 5px;
}
.item_buttons .btn {
  min-width: 13ex;
}
.item_buttons .running-indicator {
  padding-top: 4px;
  color: #5cb85c;
}
.item_buttons .kernel-name {
  padding-top: 4px;
  color: #5bc0de;
  margin-right: 7px;
  float: left;
}
[dir="rtl"] .item_buttons.pull-right {
  float: left !important;
  float: left;
}
[dir="rtl"] .item_buttons .kernel-name {
  margin-left: 7px;
  float: right;
}
.toolbar_info {
  height: 24px;
  line-height: 24px;
}
.list_item input:not([type=checkbox]) {
  padding-top: 3px;
  padding-bottom: 3px;
  height: 22px;
  line-height: 14px;
  margin: 0px;
}
.highlight_text {
  color: blue;
}
#project_name {
  display: inline-block;
  padding-left: 7px;
  margin-left: -2px;
}
#project_name > .breadcrumb {
  padding: 0px;
  margin-bottom: 0px;
  background-color: transparent;
  font-weight: bold;
}
.sort_button {
  display: inline-block;
  padding-left: 7px;
}
[dir="rtl"] .sort_button.pull-right {
  float: left !important;
  float: left;
}
#tree-selector {
  padding-right: 0px;
}
#button-select-all {
  min-width: 50px;
}
[dir="rtl"] #button-select-all.btn {
  float: right ;
}
#select-all {
  margin-left: 7px;
  margin-right: 2px;
  margin-top: 2px;
  height: 16px;
}
[dir="rtl"] #select-all.pull-left {
  float: right !important;
  float: right;
}
.menu_icon {
  margin-right: 2px;
}
.tab-content .row {
  margin-left: 0px;
  margin-right: 0px;
}
.folder_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f114";
}
.folder_icon:before.fa-pull-left {
  margin-right: .3em;
}
.folder_icon:before.fa-pull-right {
  margin-left: .3em;
}
.folder_icon:before.pull-left {
  margin-right: .3em;
}
.folder_icon:before.pull-right {
  margin-left: .3em;
}
.notebook_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f02d";
  position: relative;
  top: -1px;
}
.notebook_icon:before.fa-pull-left {
  margin-right: .3em;
}
.notebook_icon:before.fa-pull-right {
  margin-left: .3em;
}
.notebook_icon:before.pull-left {
  margin-right: .3em;
}
.notebook_icon:before.pull-right {
  margin-left: .3em;
}
.running_notebook_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f02d";
  position: relative;
  top: -1px;
  color: #5cb85c;
}
.running_notebook_icon:before.fa-pull-left {
  margin-right: .3em;
}
.running_notebook_icon:before.fa-pull-right {
  margin-left: .3em;
}
.running_notebook_icon:before.pull-left {
  margin-right: .3em;
}
.running_notebook_icon:before.pull-right {
  margin-left: .3em;
}
.file_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f016";
  position: relative;
  top: -2px;
}
.file_icon:before.fa-pull-left {
  margin-right: .3em;
}
.file_icon:before.fa-pull-right {
  margin-left: .3em;
}
.file_icon:before.pull-left {
  margin-right: .3em;
}
.file_icon:before.pull-right {
  margin-left: .3em;
}
#notebook_toolbar .pull-right {
  padding-top: 0px;
  margin-right: -1px;
}
ul#new-menu {
  left: auto;
  right: 0;
}
#new-menu .dropdown-header {
  font-size: 10px;
  border-bottom: 1px solid #e5e5e5;
  padding: 0 0 3px;
  margin: -3px 20px 0;
}
.kernel-menu-icon {
  padding-right: 12px;
  width: 24px;
  content: "\f096";
}
.kernel-menu-icon:before {
  content: "\f096";
}
.kernel-menu-icon-current:before {
  content: "\f00c";
}
#tab_content {
  padding-top: 20px;
}
#running .panel-group .panel {
  margin-top: 3px;
  margin-bottom: 1em;
}
#running .panel-group .panel .panel-heading {
  background-color: #EEE;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 7px;
  padding-right: 7px;
  line-height: 22px;
}
#running .panel-group .panel .panel-heading a:focus,
#running .panel-group .panel .panel-heading a:hover {
  text-decoration: none;
}
#running .panel-group .panel .panel-body {
  padding: 0px;
}
#running .panel-group .panel .panel-body .list_container {
  margin-top: 0px;
  margin-bottom: 0px;
  border: 0px;
  border-radius: 0px;
}
#running .panel-group .panel .panel-body .list_container .list_item {
  border-bottom: 1px solid #ddd;
}
#running .panel-group .panel .panel-body .list_container .list_item:last-child {
  border-bottom: 0px;
}
.delete-button {
  display: none;
}
.duplicate-button {
  display: none;
}
.rename-button {
  display: none;
}
.move-button {
  display: none;
}
.download-button {
  display: none;
}
.shutdown-button {
  display: none;
}
.dynamic-instructions {
  display: inline-block;
  padding-top: 4px;
}
/*!
*
* IPython text editor webapp
*
*/
.selected-keymap i.fa {
  padding: 0px 5px;
}
.selected-keymap i.fa:before {
  content: "\f00c";
}
#mode-menu {
  overflow: auto;
  max-height: 20em;
}
.edit_app #header {
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
}
.edit_app #menubar .navbar {
  /* Use a negative 1 bottom margin, so the border overlaps the border of the
    header */
  margin-bottom: -1px;
}
.dirty-indicator {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 20px;
}
.dirty-indicator.fa-pull-left {
  margin-right: .3em;
}
.dirty-indicator.fa-pull-right {
  margin-left: .3em;
}
.dirty-indicator.pull-left {
  margin-right: .3em;
}
.dirty-indicator.pull-right {
  margin-left: .3em;
}
.dirty-indicator-dirty {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 20px;
}
.dirty-indicator-dirty.fa-pull-left {
  margin-right: .3em;
}
.dirty-indicator-dirty.fa-pull-right {
  margin-left: .3em;
}
.dirty-indicator-dirty.pull-left {
  margin-right: .3em;
}
.dirty-indicator-dirty.pull-right {
  margin-left: .3em;
}
.dirty-indicator-clean {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 20px;
}
.dirty-indicator-clean.fa-pull-left {
  margin-right: .3em;
}
.dirty-indicator-clean.fa-pull-right {
  margin-left: .3em;
}
.dirty-indicator-clean.pull-left {
  margin-right: .3em;
}
.dirty-indicator-clean.pull-right {
  margin-left: .3em;
}
.dirty-indicator-clean:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f00c";
}
.dirty-indicator-clean:before.fa-pull-left {
  margin-right: .3em;
}
.dirty-indicator-clean:before.fa-pull-right {
  margin-left: .3em;
}
.dirty-indicator-clean:before.pull-left {
  margin-right: .3em;
}
.dirty-indicator-clean:before.pull-right {
  margin-left: .3em;
}
#filename {
  font-size: 16pt;
  display: table;
  padding: 0px 5px;
}
#current-mode {
  padding-left: 5px;
  padding-right: 5px;
}
#texteditor-backdrop {
  padding-top: 20px;
  padding-bottom: 20px;
}
@media not print {
  #texteditor-backdrop {
    background-color: #EEE;
  }
}
@media print {
  #texteditor-backdrop #texteditor-container .CodeMirror-gutter,
  #texteditor-backdrop #texteditor-container .CodeMirror-gutters {
    background-color: #fff;
  }
}
@media not print {
  #texteditor-backdrop #texteditor-container .CodeMirror-gutter,
  #texteditor-backdrop #texteditor-container .CodeMirror-gutters {
    background-color: #fff;
  }
}
@media not print {
  #texteditor-backdrop #texteditor-container {
    padding: 0px;
    background-color: #fff;
    -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
    box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  }
}
.CodeMirror-dialog {
  background-color: #fff;
}
/*!
*
* IPython notebook
*
*/
/* CSS font colors for translated ANSI escape sequences */
/* The color values are a mix of
   http://www.xcolors.net/dl/baskerville-ivorylight and
   http://www.xcolors.net/dl/euphrasia */
.ansi-black-fg {
  color: #3E424D;
}
.ansi-black-bg {
  background-color: #3E424D;
}
.ansi-black-intense-fg {
  color: #282C36;
}
.ansi-black-intense-bg {
  background-color: #282C36;
}
.ansi-red-fg {
  color: #E75C58;
}
.ansi-red-bg {
  background-color: #E75C58;
}
.ansi-red-intense-fg {
  color: #B22B31;
}
.ansi-red-intense-bg {
  background-color: #B22B31;
}
.ansi-green-fg {
  color: #00A250;
}
.ansi-green-bg {
  background-color: #00A250;
}
.ansi-green-intense-fg {
  color: #007427;
}
.ansi-green-intense-bg {
  background-color: #007427;
}
.ansi-yellow-fg {
  color: #DDB62B;
}
.ansi-yellow-bg {
  background-color: #DDB62B;
}
.ansi-yellow-intense-fg {
  color: #B27D12;
}
.ansi-yellow-intense-bg {
  background-color: #B27D12;
}
.ansi-blue-fg {
  color: #208FFB;
}
.ansi-blue-bg {
  background-color: #208FFB;
}
.ansi-blue-intense-fg {
  color: #0065CA;
}
.ansi-blue-intense-bg {
  background-color: #0065CA;
}
.ansi-magenta-fg {
  color: #D160C4;
}
.ansi-magenta-bg {
  background-color: #D160C4;
}
.ansi-magenta-intense-fg {
  color: #A03196;
}
.ansi-magenta-intense-bg {
  background-color: #A03196;
}
.ansi-cyan-fg {
  color: #60C6C8;
}
.ansi-cyan-bg {
  background-color: #60C6C8;
}
.ansi-cyan-intense-fg {
  color: #258F8F;
}
.ansi-cyan-intense-bg {
  background-color: #258F8F;
}
.ansi-white-fg {
  color: #C5C1B4;
}
.ansi-white-bg {
  background-color: #C5C1B4;
}
.ansi-white-intense-fg {
  color: #A1A6B2;
}
.ansi-white-intense-bg {
  background-color: #A1A6B2;
}
.ansi-default-inverse-fg {
  color: #FFFFFF;
}
.ansi-default-inverse-bg {
  background-color: #000000;
}
.ansi-bold {
  font-weight: bold;
}
.ansi-underline {
  text-decoration: underline;
}
/* The following styles are deprecated an will be removed in a future version */
.ansibold {
  font-weight: bold;
}
.ansi-inverse {
  outline: 0.5px dotted;
}
/* use dark versions for foreground, to improve visibility */
.ansiblack {
  color: black;
}
.ansired {
  color: darkred;
}
.ansigreen {
  color: darkgreen;
}
.ansiyellow {
  color: #c4a000;
}
.ansiblue {
  color: darkblue;
}
.ansipurple {
  color: darkviolet;
}
.ansicyan {
  color: steelblue;
}
.ansigray {
  color: gray;
}
/* and light for background, for the same reason */
.ansibgblack {
  background-color: black;
}
.ansibgred {
  background-color: red;
}
.ansibggreen {
  background-color: green;
}
.ansibgyellow {
  background-color: yellow;
}
.ansibgblue {
  background-color: blue;
}
.ansibgpurple {
  background-color: magenta;
}
.ansibgcyan {
  background-color: cyan;
}
.ansibggray {
  background-color: gray;
}
div.cell {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
  border-radius: 2px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  border-width: 1px;
  border-style: solid;
  border-color: transparent;
  width: 100%;
  padding: 5px;
  /* This acts as a spacer between cells, that is outside the border */
  margin: 0px;
  outline: none;
  position: relative;
  overflow: visible;
}
div.cell:before {
  position: absolute;
  display: block;
  top: -1px;
  left: -1px;
  width: 5px;
  height: calc(100% +  2px);
  content: '';
  background: transparent;
}
div.cell.jupyter-soft-selected {
  border-left-color: #E3F2FD;
  border-left-width: 1px;
  padding-left: 5px;
  border-right-color: #E3F2FD;
  border-right-width: 1px;
  background: #E3F2FD;
}
@media print {
  div.cell.jupyter-soft-selected {
    border-color: transparent;
  }
}
div.cell.selected,
div.cell.selected.jupyter-soft-selected {
  border-color: #ababab;
}
div.cell.selected:before,
div.cell.selected.jupyter-soft-selected:before {
  position: absolute;
  display: block;
  top: -1px;
  left: -1px;
  width: 5px;
  height: calc(100% +  2px);
  content: '';
  background: #42A5F5;
}
@media print {
  div.cell.selected,
  div.cell.selected.jupyter-soft-selected {
    border-color: transparent;
  }
}
.edit_mode div.cell.selected {
  border-color: #66BB6A;
}
.edit_mode div.cell.selected:before {
  position: absolute;
  display: block;
  top: -1px;
  left: -1px;
  width: 5px;
  height: calc(100% +  2px);
  content: '';
  background: #66BB6A;
}
@media print {
  .edit_mode div.cell.selected {
    border-color: transparent;
  }
}
.prompt {
  /* This needs to be wide enough for 3 digit prompt numbers: In[100]: */
  min-width: 14ex;
  /* This padding is tuned to match the padding on the CodeMirror editor. */
  padding: 0.4em;
  margin: 0px;
  font-family: monospace;
  text-align: right;
  /* This has to match that of the the CodeMirror class line-height below */
  line-height: 1.21429em;
  /* Don't highlight prompt number selection */
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  /* Use default cursor */
  cursor: default;
}
@media (max-width: 540px) {
  .prompt {
    text-align: left;
  }
}
div.inner_cell {
  min-width: 0;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
}
/* input_area and input_prompt must match in top border and margin for alignment */
div.input_area {
  border: 1px solid #cfcfcf;
  border-radius: 2px;
  background: #f7f7f7;
  line-height: 1.21429em;
}
/* This is needed so that empty prompt areas can collapse to zero height when there
   is no content in the output_subarea and the prompt. The main purpose of this is
   to make sure that empty JavaScript output_subareas have no height. */
div.prompt:empty {
  padding-top: 0;
  padding-bottom: 0;
}
div.unrecognized_cell {
  padding: 5px 5px 5px 0px;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
div.unrecognized_cell .inner_cell {
  border-radius: 2px;
  padding: 5px;
  font-weight: bold;
  color: red;
  border: 1px solid #cfcfcf;
  background: #eaeaea;
}
div.unrecognized_cell .inner_cell a {
  color: inherit;
  text-decoration: none;
}
div.unrecognized_cell .inner_cell a:hover {
  color: inherit;
  text-decoration: none;
}
@media (max-width: 540px) {
  div.unrecognized_cell > div.prompt {
    display: none;
  }
}
div.code_cell {
  /* avoid page breaking on code cells when printing */
}
@media print {
  div.code_cell {
    page-break-inside: avoid;
  }
}
/* any special styling for code cells that are currently running goes here */
div.input {
  page-break-inside: avoid;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
@media (max-width: 540px) {
  div.input {
    /* Old browsers */
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-box-align: stretch;
    display: -moz-box;
    -moz-box-orient: vertical;
    -moz-box-align: stretch;
    display: box;
    box-orient: vertical;
    box-align: stretch;
    /* Modern browsers */
    display: flex;
    flex-direction: column;
    align-items: stretch;
  }
}
/* input_area and input_prompt must match in top border and margin for alignment */
div.input_prompt {
  color: #303F9F;
  border-top: 1px solid transparent;
}
div.input_area > div.highlight {
  margin: 0.4em;
  border: none;
  padding: 0px;
  background-color: transparent;
}
div.input_area > div.highlight > pre {
  margin: 0px;
  border: none;
  padding: 0px;
  background-color: transparent;
}
/* The following gets added to the <head> if it is detected that the user has a
 * monospace font with inconsistent normal/bold/italic height.  See
 * notebookmain.js.  Such fonts will have keywords vertically offset with
 * respect to the rest of the text.  The user should select a better font.
 * See: https://github.com/ipython/ipython/issues/1503
 *
 * .CodeMirror span {
 *      vertical-align: bottom;
 * }
 */
.CodeMirror {
  line-height: 1.21429em;
  /* Changed from 1em to our global default */
  font-size: 14px;
  height: auto;
  /* Changed to auto to autogrow */
  background: none;
  /* Changed from white to allow our bg to show through */
}
.CodeMirror-scroll {
  /*  The CodeMirror docs are a bit fuzzy on if overflow-y should be hidden or visible.*/
  /*  We have found that if it is visible, vertical scrollbars appear with font size changes.*/
  overflow-y: hidden;
  overflow-x: auto;
}
.CodeMirror-lines {
  /* In CM2, this used to be 0.4em, but in CM3 it went to 4px. We need the em value because */
  /* we have set a different line-height and want this to scale with that. */
  /* Note that this should set vertical padding only, since CodeMirror assumes
       that horizontal padding will be set on CodeMirror pre */
  padding: 0.4em 0;
}
.CodeMirror-linenumber {
  padding: 0 8px 0 4px;
}
.CodeMirror-gutters {
  border-bottom-left-radius: 2px;
  border-top-left-radius: 2px;
}
.CodeMirror pre {
  /* In CM3 this went to 4px from 0 in CM2. This sets horizontal padding only,
    use .CodeMirror-lines for vertical */
  padding: 0 0.4em;
  border: 0;
  border-radius: 0;
}
.CodeMirror-cursor {
  border-left: 1.4px solid black;
}
@media screen and (min-width: 2138px) and (max-width: 4319px) {
  .CodeMirror-cursor {
    border-left: 2px solid black;
  }
}
@media screen and (min-width: 4320px) {
  .CodeMirror-cursor {
    border-left: 4px solid black;
  }
}
/*

Original style from softwaremaniacs.org (c) Ivan Sagalaev <Maniac@SoftwareManiacs.Org>
Adapted from GitHub theme

*/
.highlight-base {
  color: #000;
}
.highlight-variable {
  color: #000;
}
.highlight-variable-2 {
  color: #1a1a1a;
}
.highlight-variable-3 {
  color: #333333;
}
.highlight-string {
  color: #BA2121;
}
.highlight-comment {
  color: #408080;
  font-style: italic;
}
.highlight-number {
  color: #080;
}
.highlight-atom {
  color: #88F;
}
.highlight-keyword {
  color: #008000;
  font-weight: bold;
}
.highlight-builtin {
  color: #008000;
}
.highlight-error {
  color: #f00;
}
.highlight-operator {
  color: #AA22FF;
  font-weight: bold;
}
.highlight-meta {
  color: #AA22FF;
}
/* previously not defined, copying from default codemirror */
.highlight-def {
  color: #00f;
}
.highlight-string-2 {
  color: #f50;
}
.highlight-qualifier {
  color: #555;
}
.highlight-bracket {
  color: #997;
}
.highlight-tag {
  color: #170;
}
.highlight-attribute {
  color: #00c;
}
.highlight-header {
  color: blue;
}
.highlight-quote {
  color: #090;
}
.highlight-link {
  color: #00c;
}
/* apply the same style to codemirror */
.cm-s-ipython span.cm-keyword {
  color: #008000;
  font-weight: bold;
}
.cm-s-ipython span.cm-atom {
  color: #88F;
}
.cm-s-ipython span.cm-number {
  color: #080;
}
.cm-s-ipython span.cm-def {
  color: #00f;
}
.cm-s-ipython span.cm-variable {
  color: #000;
}
.cm-s-ipython span.cm-operator {
  color: #AA22FF;
  font-weight: bold;
}
.cm-s-ipython span.cm-variable-2 {
  color: #1a1a1a;
}
.cm-s-ipython span.cm-variable-3 {
  color: #333333;
}
.cm-s-ipython span.cm-comment {
  color: #408080;
  font-style: italic;
}
.cm-s-ipython span.cm-string {
  color: #BA2121;
}
.cm-s-ipython span.cm-string-2 {
  color: #f50;
}
.cm-s-ipython span.cm-meta {
  color: #AA22FF;
}
.cm-s-ipython span.cm-qualifier {
  color: #555;
}
.cm-s-ipython span.cm-builtin {
  color: #008000;
}
.cm-s-ipython span.cm-bracket {
  color: #997;
}
.cm-s-ipython span.cm-tag {
  color: #170;
}
.cm-s-ipython span.cm-attribute {
  color: #00c;
}
.cm-s-ipython span.cm-header {
  color: blue;
}
.cm-s-ipython span.cm-quote {
  color: #090;
}
.cm-s-ipython span.cm-link {
  color: #00c;
}
.cm-s-ipython span.cm-error {
  color: #f00;
}
.cm-s-ipython span.cm-tab {
  background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAMCAYAAAAkuj5RAAAAAXNSR0IArs4c6QAAAGFJREFUSMft1LsRQFAQheHPowAKoACx3IgEKtaEHujDjORSgWTH/ZOdnZOcM/sgk/kFFWY0qV8foQwS4MKBCS3qR6ixBJvElOobYAtivseIE120FaowJPN75GMu8j/LfMwNjh4HUpwg4LUAAAAASUVORK5CYII=);
  background-position: right;
  background-repeat: no-repeat;
}
div.output_wrapper {
  /* this position must be relative to enable descendents to be absolute within it */
  position: relative;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
  z-index: 1;
}
/* class for the output area when it should be height-limited */
div.output_scroll {
  /* ideally, this would be max-height, but FF barfs all over that */
  height: 24em;
  /* FF needs this *and the wrapper* to specify full width, or it will shrinkwrap */
  width: 100%;
  overflow: auto;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 2px 8px rgba(0, 0, 0, 0.8);
  box-shadow: inset 0 2px 8px rgba(0, 0, 0, 0.8);
  display: block;
}
/* output div while it is collapsed */
div.output_collapsed {
  margin: 0px;
  padding: 0px;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
}
div.out_prompt_overlay {
  height: 100%;
  padding: 0px 0.4em;
  position: absolute;
  border-radius: 2px;
}
div.out_prompt_overlay:hover {
  /* use inner shadow to get border that is computed the same on WebKit/FF */
  -webkit-box-shadow: inset 0 0 1px #000;
  box-shadow: inset 0 0 1px #000;
  background: rgba(240, 240, 240, 0.5);
}
div.output_prompt {
  color: #D84315;
}
/* This class is the outer container of all output sections. */
div.output_area {
  padding: 0px;
  page-break-inside: avoid;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
div.output_area .MathJax_Display {
  text-align: left !important;
}
div.output_area .rendered_html table {
  margin-left: 0;
  margin-right: 0;
}
div.output_area .rendered_html img {
  margin-left: 0;
  margin-right: 0;
}
div.output_area img,
div.output_area svg {
  max-width: 100%;
  height: auto;
}
div.output_area img.unconfined,
div.output_area svg.unconfined {
  max-width: none;
}
div.output_area .mglyph > img {
  max-width: none;
}
/* This is needed to protect the pre formating from global settings such
   as that of bootstrap */
.output {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
}
@media (max-width: 540px) {
  div.output_area {
    /* Old browsers */
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-box-align: stretch;
    display: -moz-box;
    -moz-box-orient: vertical;
    -moz-box-align: stretch;
    display: box;
    box-orient: vertical;
    box-align: stretch;
    /* Modern browsers */
    display: flex;
    flex-direction: column;
    align-items: stretch;
  }
}
div.output_area pre {
  margin: 0;
  padding: 1px 0 1px 0;
  border: 0;
  vertical-align: baseline;
  color: black;
  background-color: transparent;
  border-radius: 0;
}
/* This class is for the output subarea inside the output_area and after
   the prompt div. */
div.output_subarea {
  overflow-x: auto;
  padding: 0.4em;
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
  max-width: calc(100% - 14ex);
}
div.output_scroll div.output_subarea {
  overflow-x: visible;
}
/* The rest of the output_* classes are for special styling of the different
   output types */
/* all text output has this class: */
div.output_text {
  text-align: left;
  color: #000;
  /* This has to match that of the the CodeMirror class line-height below */
  line-height: 1.21429em;
}
/* stdout/stderr are 'text' as well as 'stream', but execute_result/error are *not* streams */
div.output_stderr {
  background: #fdd;
  /* very light red background for stderr */
}
div.output_latex {
  text-align: left;
}
/* Empty output_javascript divs should have no height */
div.output_javascript:empty {
  padding: 0;
}
.js-error {
  color: darkred;
}
/* raw_input styles */
div.raw_input_container {
  line-height: 1.21429em;
  padding-top: 5px;
}
pre.raw_input_prompt {
  /* nothing needed here. */
}
input.raw_input {
  font-family: monospace;
  font-size: inherit;
  color: inherit;
  width: auto;
  /* make sure input baseline aligns with prompt */
  vertical-align: baseline;
  /* padding + margin = 0.5em between prompt and cursor */
  padding: 0em 0.25em;
  margin: 0em 0.25em;
}
input.raw_input:focus {
  box-shadow: none;
}
p.p-space {
  margin-bottom: 10px;
}
div.output_unrecognized {
  padding: 5px;
  font-weight: bold;
  color: red;
}
div.output_unrecognized a {
  color: inherit;
  text-decoration: none;
}
div.output_unrecognized a:hover {
  color: inherit;
  text-decoration: none;
}
.rendered_html {
  color: #000;
  /* any extras will just be numbers: */
}
.rendered_html em {
  font-style: italic;
}
.rendered_html strong {
  font-weight: bold;
}
.rendered_html u {
  text-decoration: underline;
}
.rendered_html :link {
  text-decoration: underline;
}
.rendered_html :visited {
  text-decoration: underline;
}
.rendered_html h1 {
  font-size: 185.7%;
  margin: 1.08em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
}
.rendered_html h2 {
  font-size: 157.1%;
  margin: 1.27em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
}
.rendered_html h3 {
  font-size: 128.6%;
  margin: 1.55em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
}
.rendered_html h4 {
  font-size: 100%;
  margin: 2em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
}
.rendered_html h5 {
  font-size: 100%;
  margin: 2em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
  font-style: italic;
}
.rendered_html h6 {
  font-size: 100%;
  margin: 2em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
  font-style: italic;
}
.rendered_html h1:first-child {
  margin-top: 0.538em;
}
.rendered_html h2:first-child {
  margin-top: 0.636em;
}
.rendered_html h3:first-child {
  margin-top: 0.777em;
}
.rendered_html h4:first-child {
  margin-top: 1em;
}
.rendered_html h5:first-child {
  margin-top: 1em;
}
.rendered_html h6:first-child {
  margin-top: 1em;
}
.rendered_html ul:not(.list-inline),
.rendered_html ol:not(.list-inline) {
  padding-left: 2em;
}
.rendered_html ul {
  list-style: disc;
}
.rendered_html ul ul {
  list-style: square;
  margin-top: 0;
}
.rendered_html ul ul ul {
  list-style: circle;
}
.rendered_html ol {
  list-style: decimal;
}
.rendered_html ol ol {
  list-style: upper-alpha;
  margin-top: 0;
}
.rendered_html ol ol ol {
  list-style: lower-alpha;
}
.rendered_html ol ol ol ol {
  list-style: lower-roman;
}
.rendered_html ol ol ol ol ol {
  list-style: decimal;
}
.rendered_html * + ul {
  margin-top: 1em;
}
.rendered_html * + ol {
  margin-top: 1em;
}
.rendered_html hr {
  color: black;
  background-color: black;
}
.rendered_html pre {
  margin: 1em 2em;
  padding: 0px;
  background-color: #fff;
}
.rendered_html code {
  background-color: #eff0f1;
}
.rendered_html p code {
  padding: 1px 5px;
}
.rendered_html pre code {
  background-color: #fff;
}
.rendered_html pre,
.rendered_html code {
  border: 0;
  color: #000;
  font-size: 100%;
}
.rendered_html blockquote {
  margin: 1em 2em;
}
.rendered_html table {
  margin-left: auto;
  margin-right: auto;
  border: none;
  border-collapse: collapse;
  border-spacing: 0;
  color: black;
  font-size: 12px;
  table-layout: fixed;
}
.rendered_html thead {
  border-bottom: 1px solid black;
  vertical-align: bottom;
}
.rendered_html tr,
.rendered_html th,
.rendered_html td {
  text-align: right;
  vertical-align: middle;
  padding: 0.5em 0.5em;
  line-height: normal;
  white-space: normal;
  max-width: none;
  border: none;
}
.rendered_html th {
  font-weight: bold;
}
.rendered_html tbody tr:nth-child(odd) {
  background: #f5f5f5;
}
.rendered_html tbody tr:hover {
  background: rgba(66, 165, 245, 0.2);
}
.rendered_html * + table {
  margin-top: 1em;
}
.rendered_html p {
  text-align: left;
}
.rendered_html * + p {
  margin-top: 1em;
}
.rendered_html img {
  display: block;
  margin-left: auto;
  margin-right: auto;
}
.rendered_html * + img {
  margin-top: 1em;
}
.rendered_html img,
.rendered_html svg {
  max-width: 100%;
  height: auto;
}
.rendered_html img.unconfined,
.rendered_html svg.unconfined {
  max-width: none;
}
.rendered_html .alert {
  margin-bottom: initial;
}
.rendered_html * + .alert {
  margin-top: 1em;
}
[dir="rtl"] .rendered_html p {
  text-align: right;
}
div.text_cell {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
@media (max-width: 540px) {
  div.text_cell > div.prompt {
    display: none;
  }
}
div.text_cell_render {
  /*font-family: "Helvetica Neue", Arial, Helvetica, Geneva, sans-serif;*/
  outline: none;
  resize: none;
  width: inherit;
  border-style: none;
  padding: 0.5em 0.5em 0.5em 0.4em;
  color: #000;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
a.anchor-link:link {
  text-decoration: none;
  padding: 0px 20px;
  visibility: hidden;
}
h1:hover .anchor-link,
h2:hover .anchor-link,
h3:hover .anchor-link,
h4:hover .anchor-link,
h5:hover .anchor-link,
h6:hover .anchor-link {
  visibility: visible;
}
.text_cell.rendered .input_area {
  display: none;
}
.text_cell.rendered .rendered_html {
  overflow-x: auto;
  overflow-y: hidden;
}
.text_cell.rendered .rendered_html tr,
.text_cell.rendered .rendered_html th,
.text_cell.rendered .rendered_html td {
  max-width: none;
}
.text_cell.unrendered .text_cell_render {
  display: none;
}
.text_cell .dropzone .input_area {
  border: 2px dashed #bababa;
  margin: -1px;
}
.cm-header-1,
.cm-header-2,
.cm-header-3,
.cm-header-4,
.cm-header-5,
.cm-header-6 {
  font-weight: bold;
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
}
.cm-header-1 {
  font-size: 185.7%;
}
.cm-header-2 {
  font-size: 157.1%;
}
.cm-header-3 {
  font-size: 128.6%;
}
.cm-header-4 {
  font-size: 110%;
}
.cm-header-5 {
  font-size: 100%;
  font-style: italic;
}
.cm-header-6 {
  font-size: 100%;
  font-style: italic;
}
/*!
*
* IPython notebook webapp
*
*/
@media (max-width: 767px) {
  .notebook_app {
    padding-left: 0px;
    padding-right: 0px;
  }
}
#ipython-main-app {
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  height: 100%;
}
div#notebook_panel {
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  height: 100%;
}
div#notebook {
  font-size: 14px;
  line-height: 20px;
  overflow-y: hidden;
  overflow-x: auto;
  width: 100%;
  /* This spaces the page away from the edge of the notebook area */
  padding-top: 20px;
  margin: 0px;
  outline: none;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  min-height: 100%;
}
@media not print {
  #notebook-container {
    padding: 15px;
    background-color: #fff;
    min-height: 0;
    -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
    box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  }
}
@media print {
  #notebook-container {
    width: 100%;
  }
}
div.ui-widget-content {
  border: 1px solid #ababab;
  outline: none;
}
pre.dialog {
  background-color: #f7f7f7;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0.4em;
  padding-left: 2em;
}
p.dialog {
  padding: 0.2em;
}
/* Word-wrap output correctly.  This is the CSS3 spelling, though Firefox seems
   to not honor it correctly.  Webkit browsers (Chrome, rekonq, Safari) do.
 */
pre,
code,
kbd,
samp {
  white-space: pre-wrap;
}
#fonttest {
  font-family: monospace;
}
p {
  margin-bottom: 0;
}
.end_space {
  min-height: 100px;
  transition: height .2s ease;
}
.notebook_app > #header {
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
}
@media not print {
  .notebook_app {
    background-color: #EEE;
  }
}
kbd {
  border-style: solid;
  border-width: 1px;
  box-shadow: none;
  margin: 2px;
  padding-left: 2px;
  padding-right: 2px;
  padding-top: 1px;
  padding-bottom: 1px;
}
.jupyter-keybindings {
  padding: 1px;
  line-height: 24px;
  border-bottom: 1px solid gray;
}
.jupyter-keybindings input {
  margin: 0;
  padding: 0;
  border: none;
}
.jupyter-keybindings i {
  padding: 6px;
}
.well code {
  background-color: #ffffff;
  border-color: #ababab;
  border-width: 1px;
  border-style: solid;
  padding: 2px;
  padding-top: 1px;
  padding-bottom: 1px;
}
/* CSS for the cell toolbar */
.celltoolbar {
  border: thin solid #CFCFCF;
  border-bottom: none;
  background: #EEE;
  border-radius: 2px 2px 0px 0px;
  width: 100%;
  height: 29px;
  padding-right: 4px;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
  /* Old browsers */
  -webkit-box-pack: end;
  -moz-box-pack: end;
  box-pack: end;
  /* Modern browsers */
  justify-content: flex-end;
  display: -webkit-flex;
}
@media print {
  .celltoolbar {
    display: none;
  }
}
.ctb_hideshow {
  display: none;
  vertical-align: bottom;
}
/* ctb_show is added to the ctb_hideshow div to show the cell toolbar.
   Cell toolbars are only shown when the ctb_global_show class is also set.
*/
.ctb_global_show .ctb_show.ctb_hideshow {
  display: block;
}
.ctb_global_show .ctb_show + .input_area,
.ctb_global_show .ctb_show + div.text_cell_input,
.ctb_global_show .ctb_show ~ div.text_cell_render {
  border-top-right-radius: 0px;
  border-top-left-radius: 0px;
}
.ctb_global_show .ctb_show ~ div.text_cell_render {
  border: 1px solid #cfcfcf;
}
.celltoolbar {
  font-size: 87%;
  padding-top: 3px;
}
.celltoolbar select {
  display: block;
  width: 100%;
  height: 32px;
  padding: 6px 12px;
  font-size: 13px;
  line-height: 1.42857143;
  color: #555555;
  background-color: #fff;
  background-image: none;
  border: 1px solid #ccc;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  -webkit-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  -o-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
  width: inherit;
  font-size: inherit;
  height: 22px;
  padding: 0px;
  display: inline-block;
}
.celltoolbar select:focus {
  border-color: #66afe9;
  outline: 0;
  -webkit-box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
  box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
}
.celltoolbar select::-moz-placeholder {
  color: #999;
  opacity: 1;
}
.celltoolbar select:-ms-input-placeholder {
  color: #999;
}
.celltoolbar select::-webkit-input-placeholder {
  color: #999;
}
.celltoolbar select::-ms-expand {
  border: 0;
  background-color: transparent;
}
.celltoolbar select[disabled],
.celltoolbar select[readonly],
fieldset[disabled] .celltoolbar select {
  background-color: #eeeeee;
  opacity: 1;
}
.celltoolbar select[disabled],
fieldset[disabled] .celltoolbar select {
  cursor: not-allowed;
}
textarea.celltoolbar select {
  height: auto;
}
select.celltoolbar select {
  height: 30px;
  line-height: 30px;
}
textarea.celltoolbar select,
select[multiple].celltoolbar select {
  height: auto;
}
.celltoolbar label {
  margin-left: 5px;
  margin-right: 5px;
}
.tags_button_container {
  width: 100%;
  display: flex;
}
.tag-container {
  display: flex;
  flex-direction: row;
  flex-grow: 1;
  overflow: hidden;
  position: relative;
}
.tag-container > * {
  margin: 0 4px;
}
.remove-tag-btn {
  margin-left: 4px;
}
.tags-input {
  display: flex;
}
.cell-tag:last-child:after {
  content: "";
  position: absolute;
  right: 0;
  width: 40px;
  height: 100%;
  /* Fade to background color of cell toolbar */
  background: linear-gradient(to right, rgba(0, 0, 0, 0), #EEE);
}
.tags-input > * {
  margin-left: 4px;
}
.cell-tag,
.tags-input input,
.tags-input button {
  display: block;
  width: 100%;
  height: 32px;
  padding: 6px 12px;
  font-size: 13px;
  line-height: 1.42857143;
  color: #555555;
  background-color: #fff;
  background-image: none;
  border: 1px solid #ccc;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  -webkit-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  -o-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
  box-shadow: none;
  width: inherit;
  font-size: inherit;
  height: 22px;
  line-height: 22px;
  padding: 0px 4px;
  display: inline-block;
}
.cell-tag:focus,
.tags-input input:focus,
.tags-input button:focus {
  border-color: #66afe9;
  outline: 0;
  -webkit-box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
  box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
}
.cell-tag::-moz-placeholder,
.tags-input input::-moz-placeholder,
.tags-input button::-moz-placeholder {
  color: #999;
  opacity: 1;
}
.cell-tag:-ms-input-placeholder,
.tags-input input:-ms-input-placeholder,
.tags-input button:-ms-input-placeholder {
  color: #999;
}
.cell-tag::-webkit-input-placeholder,
.tags-input input::-webkit-input-placeholder,
.tags-input button::-webkit-input-placeholder {
  color: #999;
}
.cell-tag::-ms-expand,
.tags-input input::-ms-expand,
.tags-input button::-ms-expand {
  border: 0;
  background-color: transparent;
}
.cell-tag[disabled],
.tags-input input[disabled],
.tags-input button[disabled],
.cell-tag[readonly],
.tags-input input[readonly],
.tags-input button[readonly],
fieldset[disabled] .cell-tag,
fieldset[disabled] .tags-input input,
fieldset[disabled] .tags-input button {
  background-color: #eeeeee;
  opacity: 1;
}
.cell-tag[disabled],
.tags-input input[disabled],
.tags-input button[disabled],
fieldset[disabled] .cell-tag,
fieldset[disabled] .tags-input input,
fieldset[disabled] .tags-input button {
  cursor: not-allowed;
}
textarea.cell-tag,
textarea.tags-input input,
textarea.tags-input button {
  height: auto;
}
select.cell-tag,
select.tags-input input,
select.tags-input button {
  height: 30px;
  line-height: 30px;
}
textarea.cell-tag,
textarea.tags-input input,
textarea.tags-input button,
select[multiple].cell-tag,
select[multiple].tags-input input,
select[multiple].tags-input button {
  height: auto;
}
.cell-tag,
.tags-input button {
  padding: 0px 4px;
}
.cell-tag {
  background-color: #fff;
  white-space: nowrap;
}
.tags-input input[type=text]:focus {
  outline: none;
  box-shadow: none;
  border-color: #ccc;
}
.completions {
  position: absolute;
  z-index: 110;
  overflow: hidden;
  border: 1px solid #ababab;
  border-radius: 2px;
  -webkit-box-shadow: 0px 6px 10px -1px #adadad;
  box-shadow: 0px 6px 10px -1px #adadad;
  line-height: 1;
}
.completions select {
  background: white;
  outline: none;
  border: none;
  padding: 0px;
  margin: 0px;
  overflow: auto;
  font-family: monospace;
  font-size: 110%;
  color: #000;
  width: auto;
}
.completions select option.context {
  color: #286090;
}
#kernel_logo_widget .current_kernel_logo {
  display: none;
  margin-top: -1px;
  margin-bottom: -1px;
  width: 32px;
  height: 32px;
}
[dir="rtl"] #kernel_logo_widget {
  float: left !important;
  float: left;
}
.modal .modal-body .move-path {
  display: flex;
  flex-direction: row;
  justify-content: space;
  align-items: center;
}
.modal .modal-body .move-path .server-root {
  padding-right: 20px;
}
.modal .modal-body .move-path .path-input {
  flex: 1;
}
#menubar {
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  margin-top: 1px;
}
#menubar .navbar {
  border-top: 1px;
  border-radius: 0px 0px 2px 2px;
  margin-bottom: 0px;
}
#menubar .navbar-toggle {
  float: left;
  padding-top: 7px;
  padding-bottom: 7px;
  border: none;
}
#menubar .navbar-collapse {
  clear: left;
}
[dir="rtl"] #menubar .navbar-toggle {
  float: right;
}
[dir="rtl"] #menubar .navbar-collapse {
  clear: right;
}
[dir="rtl"] #menubar .navbar-nav {
  float: right;
}
[dir="rtl"] #menubar .nav {
  padding-right: 0px;
}
[dir="rtl"] #menubar .navbar-nav > li {
  float: right;
}
[dir="rtl"] #menubar .navbar-right {
  float: left !important;
}
[dir="rtl"] ul.dropdown-menu {
  text-align: right;
  left: auto;
}
[dir="rtl"] ul#new-menu.dropdown-menu {
  right: auto;
  left: 0;
}
.nav-wrapper {
  border-bottom: 1px solid #e7e7e7;
}
i.menu-icon {
  padding-top: 4px;
}
[dir="rtl"] i.menu-icon.pull-right {
  float: left !important;
  float: left;
}
ul#help_menu li a {
  overflow: hidden;
  padding-right: 2.2em;
}
ul#help_menu li a i {
  margin-right: -1.2em;
}
[dir="rtl"] ul#help_menu li a {
  padding-left: 2.2em;
}
[dir="rtl"] ul#help_menu li a i {
  margin-right: 0;
  margin-left: -1.2em;
}
[dir="rtl"] ul#help_menu li a i.pull-right {
  float: left !important;
  float: left;
}
.dropdown-submenu {
  position: relative;
}
.dropdown-submenu > .dropdown-menu {
  top: 0;
  left: 100%;
  margin-top: -6px;
  margin-left: -1px;
}
[dir="rtl"] .dropdown-submenu > .dropdown-menu {
  right: 100%;
  margin-right: -1px;
}
.dropdown-submenu:hover > .dropdown-menu {
  display: block;
}
.dropdown-submenu > a:after {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: block;
  content: "\f0da";
  float: right;
  color: #333333;
  margin-top: 2px;
  margin-right: -10px;
}
.dropdown-submenu > a:after.fa-pull-left {
  margin-right: .3em;
}
.dropdown-submenu > a:after.fa-pull-right {
  margin-left: .3em;
}
.dropdown-submenu > a:after.pull-left {
  margin-right: .3em;
}
.dropdown-submenu > a:after.pull-right {
  margin-left: .3em;
}
[dir="rtl"] .dropdown-submenu > a:after {
  float: left;
  content: "\f0d9";
  margin-right: 0;
  margin-left: -10px;
}
.dropdown-submenu:hover > a:after {
  color: #262626;
}
.dropdown-submenu.pull-left {
  float: none;
}
.dropdown-submenu.pull-left > .dropdown-menu {
  left: -100%;
  margin-left: 10px;
}
#notification_area {
  float: right !important;
  float: right;
  z-index: 10;
}
[dir="rtl"] #notification_area {
  float: left !important;
  float: left;
}
.indicator_area {
  float: right !important;
  float: right;
  color: #777;
  margin-left: 5px;
  margin-right: 5px;
  width: 11px;
  z-index: 10;
  text-align: center;
  width: auto;
}
[dir="rtl"] .indicator_area {
  float: left !important;
  float: left;
}
#kernel_indicator {
  float: right !important;
  float: right;
  color: #777;
  margin-left: 5px;
  margin-right: 5px;
  width: 11px;
  z-index: 10;
  text-align: center;
  width: auto;
  border-left: 1px solid;
}
#kernel_indicator .kernel_indicator_name {
  padding-left: 5px;
  padding-right: 5px;
}
[dir="rtl"] #kernel_indicator {
  float: left !important;
  float: left;
  border-left: 0;
  border-right: 1px solid;
}
#modal_indicator {
  float: right !important;
  float: right;
  color: #777;
  margin-left: 5px;
  margin-right: 5px;
  width: 11px;
  z-index: 10;
  text-align: center;
  width: auto;
}
[dir="rtl"] #modal_indicator {
  float: left !important;
  float: left;
}
#readonly-indicator {
  float: right !important;
  float: right;
  color: #777;
  margin-left: 5px;
  margin-right: 5px;
  width: 11px;
  z-index: 10;
  text-align: center;
  width: auto;
  margin-top: 2px;
  margin-bottom: 0px;
  margin-left: 0px;
  margin-right: 0px;
  display: none;
}
.modal_indicator:before {
  width: 1.28571429em;
  text-align: center;
}
.edit_mode .modal_indicator:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f040";
}
.edit_mode .modal_indicator:before.fa-pull-left {
  margin-right: .3em;
}
.edit_mode .modal_indicator:before.fa-pull-right {
  margin-left: .3em;
}
.edit_mode .modal_indicator:before.pull-left {
  margin-right: .3em;
}
.edit_mode .modal_indicator:before.pull-right {
  margin-left: .3em;
}
.command_mode .modal_indicator:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: ' ';
}
.command_mode .modal_indicator:before.fa-pull-left {
  margin-right: .3em;
}
.command_mode .modal_indicator:before.fa-pull-right {
  margin-left: .3em;
}
.command_mode .modal_indicator:before.pull-left {
  margin-right: .3em;
}
.command_mode .modal_indicator:before.pull-right {
  margin-left: .3em;
}
.kernel_idle_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f10c";
}
.kernel_idle_icon:before.fa-pull-left {
  margin-right: .3em;
}
.kernel_idle_icon:before.fa-pull-right {
  margin-left: .3em;
}
.kernel_idle_icon:before.pull-left {
  margin-right: .3em;
}
.kernel_idle_icon:before.pull-right {
  margin-left: .3em;
}
.kernel_busy_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f111";
}
.kernel_busy_icon:before.fa-pull-left {
  margin-right: .3em;
}
.kernel_busy_icon:before.fa-pull-right {
  margin-left: .3em;
}
.kernel_busy_icon:before.pull-left {
  margin-right: .3em;
}
.kernel_busy_icon:before.pull-right {
  margin-left: .3em;
}
.kernel_dead_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f1e2";
}
.kernel_dead_icon:before.fa-pull-left {
  margin-right: .3em;
}
.kernel_dead_icon:before.fa-pull-right {
  margin-left: .3em;
}
.kernel_dead_icon:before.pull-left {
  margin-right: .3em;
}
.kernel_dead_icon:before.pull-right {
  margin-left: .3em;
}
.kernel_disconnected_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f127";
}
.kernel_disconnected_icon:before.fa-pull-left {
  margin-right: .3em;
}
.kernel_disconnected_icon:before.fa-pull-right {
  margin-left: .3em;
}
.kernel_disconnected_icon:before.pull-left {
  margin-right: .3em;
}
.kernel_disconnected_icon:before.pull-right {
  margin-left: .3em;
}
.notification_widget {
  color: #777;
  z-index: 10;
  background: rgba(240, 240, 240, 0.5);
  margin-right: 4px;
  color: #333;
  background-color: #fff;
  border-color: #ccc;
}
.notification_widget:focus,
.notification_widget.focus {
  color: #333;
  background-color: #e6e6e6;
  border-color: #8c8c8c;
}
.notification_widget:hover {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
.notification_widget:active,
.notification_widget.active,
.open > .dropdown-toggle.notification_widget {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
.notification_widget:active:hover,
.notification_widget.active:hover,
.open > .dropdown-toggle.notification_widget:hover,
.notification_widget:active:focus,
.notification_widget.active:focus,
.open > .dropdown-toggle.notification_widget:focus,
.notification_widget:active.focus,
.notification_widget.active.focus,
.open > .dropdown-toggle.notification_widget.focus {
  color: #333;
  background-color: #d4d4d4;
  border-color: #8c8c8c;
}
.notification_widget:active,
.notification_widget.active,
.open > .dropdown-toggle.notification_widget {
  background-image: none;
}
.notification_widget.disabled:hover,
.notification_widget[disabled]:hover,
fieldset[disabled] .notification_widget:hover,
.notification_widget.disabled:focus,
.notification_widget[disabled]:focus,
fieldset[disabled] .notification_widget:focus,
.notification_widget.disabled.focus,
.notification_widget[disabled].focus,
fieldset[disabled] .notification_widget.focus {
  background-color: #fff;
  border-color: #ccc;
}
.notification_widget .badge {
  color: #fff;
  background-color: #333;
}
.notification_widget.warning {
  color: #fff;
  background-color: #f0ad4e;
  border-color: #eea236;
}
.notification_widget.warning:focus,
.notification_widget.warning.focus {
  color: #fff;
  background-color: #ec971f;
  border-color: #985f0d;
}
.notification_widget.warning:hover {
  color: #fff;
  background-color: #ec971f;
  border-color: #d58512;
}
.notification_widget.warning:active,
.notification_widget.warning.active,
.open > .dropdown-toggle.notification_widget.warning {
  color: #fff;
  background-color: #ec971f;
  border-color: #d58512;
}
.notification_widget.warning:active:hover,
.notification_widget.warning.active:hover,
.open > .dropdown-toggle.notification_widget.warning:hover,
.notification_widget.warning:active:focus,
.notification_widget.warning.active:focus,
.open > .dropdown-toggle.notification_widget.warning:focus,
.notification_widget.warning:active.focus,
.notification_widget.warning.active.focus,
.open > .dropdown-toggle.notification_widget.warning.focus {
  color: #fff;
  background-color: #d58512;
  border-color: #985f0d;
}
.notification_widget.warning:active,
.notification_widget.warning.active,
.open > .dropdown-toggle.notification_widget.warning {
  background-image: none;
}
.notification_widget.warning.disabled:hover,
.notification_widget.warning[disabled]:hover,
fieldset[disabled] .notification_widget.warning:hover,
.notification_widget.warning.disabled:focus,
.notification_widget.warning[disabled]:focus,
fieldset[disabled] .notification_widget.warning:focus,
.notification_widget.warning.disabled.focus,
.notification_widget.warning[disabled].focus,
fieldset[disabled] .notification_widget.warning.focus {
  background-color: #f0ad4e;
  border-color: #eea236;
}
.notification_widget.warning .badge {
  color: #f0ad4e;
  background-color: #fff;
}
.notification_widget.success {
  color: #fff;
  background-color: #5cb85c;
  border-color: #4cae4c;
}
.notification_widget.success:focus,
.notification_widget.success.focus {
  color: #fff;
  background-color: #449d44;
  border-color: #255625;
}
.notification_widget.success:hover {
  color: #fff;
  background-color: #449d44;
  border-color: #398439;
}
.notification_widget.success:active,
.notification_widget.success.active,
.open > .dropdown-toggle.notification_widget.success {
  color: #fff;
  background-color: #449d44;
  border-color: #398439;
}
.notification_widget.success:active:hover,
.notification_widget.success.active:hover,
.open > .dropdown-toggle.notification_widget.success:hover,
.notification_widget.success:active:focus,
.notification_widget.success.active:focus,
.open > .dropdown-toggle.notification_widget.success:focus,
.notification_widget.success:active.focus,
.notification_widget.success.active.focus,
.open > .dropdown-toggle.notification_widget.success.focus {
  color: #fff;
  background-color: #398439;
  border-color: #255625;
}
.notification_widget.success:active,
.notification_widget.success.active,
.open > .dropdown-toggle.notification_widget.success {
  background-image: none;
}
.notification_widget.success.disabled:hover,
.notification_widget.success[disabled]:hover,
fieldset[disabled] .notification_widget.success:hover,
.notification_widget.success.disabled:focus,
.notification_widget.success[disabled]:focus,
fieldset[disabled] .notification_widget.success:focus,
.notification_widget.success.disabled.focus,
.notification_widget.success[disabled].focus,
fieldset[disabled] .notification_widget.success.focus {
  background-color: #5cb85c;
  border-color: #4cae4c;
}
.notification_widget.success .badge {
  color: #5cb85c;
  background-color: #fff;
}
.notification_widget.info {
  color: #fff;
  background-color: #5bc0de;
  border-color: #46b8da;
}
.notification_widget.info:focus,
.notification_widget.info.focus {
  color: #fff;
  background-color: #31b0d5;
  border-color: #1b6d85;
}
.notification_widget.info:hover {
  color: #fff;
  background-color: #31b0d5;
  border-color: #269abc;
}
.notification_widget.info:active,
.notification_widget.info.active,
.open > .dropdown-toggle.notification_widget.info {
  color: #fff;
  background-color: #31b0d5;
  border-color: #269abc;
}
.notification_widget.info:active:hover,
.notification_widget.info.active:hover,
.open > .dropdown-toggle.notification_widget.info:hover,
.notification_widget.info:active:focus,
.notification_widget.info.active:focus,
.open > .dropdown-toggle.notification_widget.info:focus,
.notification_widget.info:active.focus,
.notification_widget.info.active.focus,
.open > .dropdown-toggle.notification_widget.info.focus {
  color: #fff;
  background-color: #269abc;
  border-color: #1b6d85;
}
.notification_widget.info:active,
.notification_widget.info.active,
.open > .dropdown-toggle.notification_widget.info {
  background-image: none;
}
.notification_widget.info.disabled:hover,
.notification_widget.info[disabled]:hover,
fieldset[disabled] .notification_widget.info:hover,
.notification_widget.info.disabled:focus,
.notification_widget.info[disabled]:focus,
fieldset[disabled] .notification_widget.info:focus,
.notification_widget.info.disabled.focus,
.notification_widget.info[disabled].focus,
fieldset[disabled] .notification_widget.info.focus {
  background-color: #5bc0de;
  border-color: #46b8da;
}
.notification_widget.info .badge {
  color: #5bc0de;
  background-color: #fff;
}
.notification_widget.danger {
  color: #fff;
  background-color: #d9534f;
  border-color: #d43f3a;
}
.notification_widget.danger:focus,
.notification_widget.danger.focus {
  color: #fff;
  background-color: #c9302c;
  border-color: #761c19;
}
.notification_widget.danger:hover {
  color: #fff;
  background-color: #c9302c;
  border-color: #ac2925;
}
.notification_widget.danger:active,
.notification_widget.danger.active,
.open > .dropdown-toggle.notification_widget.danger {
  color: #fff;
  background-color: #c9302c;
  border-color: #ac2925;
}
.notification_widget.danger:active:hover,
.notification_widget.danger.active:hover,
.open > .dropdown-toggle.notification_widget.danger:hover,
.notification_widget.danger:active:focus,
.notification_widget.danger.active:focus,
.open > .dropdown-toggle.notification_widget.danger:focus,
.notification_widget.danger:active.focus,
.notification_widget.danger.active.focus,
.open > .dropdown-toggle.notification_widget.danger.focus {
  color: #fff;
  background-color: #ac2925;
  border-color: #761c19;
}
.notification_widget.danger:active,
.notification_widget.danger.active,
.open > .dropdown-toggle.notification_widget.danger {
  background-image: none;
}
.notification_widget.danger.disabled:hover,
.notification_widget.danger[disabled]:hover,
fieldset[disabled] .notification_widget.danger:hover,
.notification_widget.danger.disabled:focus,
.notification_widget.danger[disabled]:focus,
fieldset[disabled] .notification_widget.danger:focus,
.notification_widget.danger.disabled.focus,
.notification_widget.danger[disabled].focus,
fieldset[disabled] .notification_widget.danger.focus {
  background-color: #d9534f;
  border-color: #d43f3a;
}
.notification_widget.danger .badge {
  color: #d9534f;
  background-color: #fff;
}
div#pager {
  background-color: #fff;
  font-size: 14px;
  line-height: 20px;
  overflow: hidden;
  display: none;
  position: fixed;
  bottom: 0px;
  width: 100%;
  max-height: 50%;
  padding-top: 8px;
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  /* Display over codemirror */
  z-index: 100;
  /* Hack which prevents jquery ui resizable from changing top. */
  top: auto !important;
}
div#pager pre {
  line-height: 1.21429em;
  color: #000;
  background-color: #f7f7f7;
  padding: 0.4em;
}
div#pager #pager-button-area {
  position: absolute;
  top: 8px;
  right: 20px;
}
div#pager #pager-contents {
  position: relative;
  overflow: auto;
  width: 100%;
  height: 100%;
}
div#pager #pager-contents #pager-container {
  position: relative;
  padding: 15px 0px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
div#pager .ui-resizable-handle {
  top: 0px;
  height: 8px;
  background: #f7f7f7;
  border-top: 1px solid #cfcfcf;
  border-bottom: 1px solid #cfcfcf;
  /* This injects handle bars (a short, wide = symbol) for 
        the resize handle. */
}
div#pager .ui-resizable-handle::after {
  content: '';
  top: 2px;
  left: 50%;
  height: 3px;
  width: 30px;
  margin-left: -15px;
  position: absolute;
  border-top: 1px solid #cfcfcf;
}
.quickhelp {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
  line-height: 1.8em;
}
.shortcut_key {
  display: inline-block;
  width: 21ex;
  text-align: right;
  font-family: monospace;
}
.shortcut_descr {
  display: inline-block;
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
}
span.save_widget {
  height: 30px;
  margin-top: 4px;
  display: flex;
  justify-content: flex-start;
  align-items: baseline;
  width: 50%;
  flex: 1;
}
span.save_widget span.filename {
  height: 100%;
  line-height: 1em;
  margin-left: 16px;
  border: none;
  font-size: 146.5%;
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
  border-radius: 2px;
}
span.save_widget span.filename:hover {
  background-color: #e6e6e6;
}
[dir="rtl"] span.save_widget.pull-left {
  float: right !important;
  float: right;
}
[dir="rtl"] span.save_widget span.filename {
  margin-left: 0;
  margin-right: 16px;
}
span.checkpoint_status,
span.autosave_status {
  font-size: small;
  white-space: nowrap;
  padding: 0 5px;
}
@media (max-width: 767px) {
  span.save_widget {
    font-size: small;
    padding: 0 0 0 5px;
  }
  span.checkpoint_status,
  span.autosave_status {
    display: none;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  span.checkpoint_status {
    display: none;
  }
  span.autosave_status {
    font-size: x-small;
  }
}
.toolbar {
  padding: 0px;
  margin-left: -5px;
  margin-top: 2px;
  margin-bottom: 5px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
.toolbar select,
.toolbar label {
  width: auto;
  vertical-align: middle;
  margin-right: 2px;
  margin-bottom: 0px;
  display: inline;
  font-size: 92%;
  margin-left: 0.3em;
  margin-right: 0.3em;
  padding: 0px;
  padding-top: 3px;
}
.toolbar .btn {
  padding: 2px 8px;
}
.toolbar .btn-group {
  margin-top: 0px;
  margin-left: 5px;
}
.toolbar-btn-label {
  margin-left: 6px;
}
#maintoolbar {
  margin-bottom: -3px;
  margin-top: -8px;
  border: 0px;
  min-height: 27px;
  margin-left: 0px;
  padding-top: 11px;
  padding-bottom: 3px;
}
#maintoolbar .navbar-text {
  float: none;
  vertical-align: middle;
  text-align: right;
  margin-left: 5px;
  margin-right: 0px;
  margin-top: 0px;
}
.select-xs {
  height: 24px;
}
[dir="rtl"] .btn-group > .btn,
.btn-group-vertical > .btn {
  float: right;
}
.pulse,
.dropdown-menu > li > a.pulse,
li.pulse > a.dropdown-toggle,
li.pulse.open > a.dropdown-toggle {
  background-color: #F37626;
  color: white;
}
/**
 * Primary styles
 *
 * Author: Jupyter Development Team
 */
/** WARNING IF YOU ARE EDITTING THIS FILE, if this is a .css file, It has a lot
 * of chance of beeing generated from the ../less/[samename].less file, you can
 * try to get back the less file by reverting somme commit in history
 **/
/*
 * We'll try to get something pretty, so we
 * have some strange css to have the scroll bar on
 * the left with fix button on the top right of the tooltip
 */
@-moz-keyframes fadeOut {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
@-webkit-keyframes fadeOut {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
@-moz-keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
@-webkit-keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
/*properties of tooltip after "expand"*/
.bigtooltip {
  overflow: auto;
  height: 200px;
  -webkit-transition-property: height;
  -webkit-transition-duration: 500ms;
  -moz-transition-property: height;
  -moz-transition-duration: 500ms;
  transition-property: height;
  transition-duration: 500ms;
}
/*properties of tooltip before "expand"*/
.smalltooltip {
  -webkit-transition-property: height;
  -webkit-transition-duration: 500ms;
  -moz-transition-property: height;
  -moz-transition-duration: 500ms;
  transition-property: height;
  transition-duration: 500ms;
  text-overflow: ellipsis;
  overflow: hidden;
  height: 80px;
}
.tooltipbuttons {
  position: absolute;
  padding-right: 15px;
  top: 0px;
  right: 0px;
}
.tooltiptext {
  /*avoid the button to overlap on some docstring*/
  padding-right: 30px;
}
.ipython_tooltip {
  max-width: 700px;
  /*fade-in animation when inserted*/
  -webkit-animation: fadeOut 400ms;
  -moz-animation: fadeOut 400ms;
  animation: fadeOut 400ms;
  -webkit-animation: fadeIn 400ms;
  -moz-animation: fadeIn 400ms;
  animation: fadeIn 400ms;
  vertical-align: middle;
  background-color: #f7f7f7;
  overflow: visible;
  border: #ababab 1px solid;
  outline: none;
  padding: 3px;
  margin: 0px;
  padding-left: 7px;
  font-family: monospace;
  min-height: 50px;
  -moz-box-shadow: 0px 6px 10px -1px #adadad;
  -webkit-box-shadow: 0px 6px 10px -1px #adadad;
  box-shadow: 0px 6px 10px -1px #adadad;
  border-radius: 2px;
  position: absolute;
  z-index: 1000;
}
.ipython_tooltip a {
  float: right;
}
.ipython_tooltip .tooltiptext pre {
  border: 0;
  border-radius: 0;
  font-size: 100%;
  background-color: #f7f7f7;
}
.pretooltiparrow {
  left: 0px;
  margin: 0px;
  top: -16px;
  width: 40px;
  height: 16px;
  overflow: hidden;
  position: absolute;
}
.pretooltiparrow:before {
  background-color: #f7f7f7;
  border: 1px #ababab solid;
  z-index: 11;
  content: "";
  position: absolute;
  left: 15px;
  top: 10px;
  width: 25px;
  height: 25px;
  -webkit-transform: rotate(45deg);
  -moz-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  -o-transform: rotate(45deg);
}
ul.typeahead-list i {
  margin-left: -10px;
  width: 18px;
}
[dir="rtl"] ul.typeahead-list i {
  margin-left: 0;
  margin-right: -10px;
}
ul.typeahead-list {
  max-height: 80vh;
  overflow: auto;
}
ul.typeahead-list > li > a {
  /** Firefox bug **/
  /* see https://github.com/jupyter/notebook/issues/559 */
  white-space: normal;
}
ul.typeahead-list  > li > a.pull-right {
  float: left !important;
  float: left;
}
[dir="rtl"] .typeahead-list {
  text-align: right;
}
.cmd-palette .modal-body {
  padding: 7px;
}
.cmd-palette form {
  background: white;
}
.cmd-palette input {
  outline: none;
}
.no-shortcut {
  min-width: 20px;
  color: transparent;
}
[dir="rtl"] .no-shortcut.pull-right {
  float: left !important;
  float: left;
}
[dir="rtl"] .command-shortcut.pull-right {
  float: left !important;
  float: left;
}
.command-shortcut:before {
  content: "(command mode)";
  padding-right: 3px;
  color: #777777;
}
.edit-shortcut:before {
  content: "(edit)";
  padding-right: 3px;
  color: #777777;
}
[dir="rtl"] .edit-shortcut.pull-right {
  float: left !important;
  float: left;
}
#find-and-replace #replace-preview .match,
#find-and-replace #replace-preview .insert {
  background-color: #BBDEFB;
  border-color: #90CAF9;
  border-style: solid;
  border-width: 1px;
  border-radius: 0px;
}
[dir="ltr"] #find-and-replace .input-group-btn + .form-control {
  border-left: none;
}
[dir="rtl"] #find-and-replace .input-group-btn + .form-control {
  border-right: none;
}
#find-and-replace #replace-preview .replace .match {
  background-color: #FFCDD2;
  border-color: #EF9A9A;
  border-radius: 0px;
}
#find-and-replace #replace-preview .replace .insert {
  background-color: #C8E6C9;
  border-color: #A5D6A7;
  border-radius: 0px;
}
#find-and-replace #replace-preview {
  max-height: 60vh;
  overflow: auto;
}
#find-and-replace #replace-preview pre {
  padding: 5px 10px;
}
.terminal-app {
  background: #EEE;
}
.terminal-app #header {
  background: #fff;
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
}
.terminal-app .terminal {
  width: 100%;
  float: left;
  font-family: monospace;
  color: white;
  background: black;
  padding: 0.4em;
  border-radius: 2px;
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.4);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.4);
}
.terminal-app .terminal,
.terminal-app .terminal dummy-screen {
  line-height: 1em;
  font-size: 14px;
}
.terminal-app .terminal .xterm-rows {
  padding: 10px;
}
.terminal-app .terminal-cursor {
  color: black;
  background: white;
}
.terminal-app #terminado-container {
  margin-top: 20px;
}
/*# sourceMappingURL=style.min.css.map */
    </style>
<style type="text/css">
    .highlight .hll { background-color: #ffffcc }
.highlight  { background: #f8f8f8; }
.highlight .c { color: #408080; font-style: italic } /* Comment */
.highlight .err { border: 1px solid #FF0000 } /* Error */
.highlight .k { color: #008000; font-weight: bold } /* Keyword */
.highlight .o { color: #666666 } /* Operator */
.highlight .ch { color: #408080; font-style: italic } /* Comment.Hashbang */
.highlight .cm { color: #408080; font-style: italic } /* Comment.Multiline */
.highlight .cp { color: #BC7A00 } /* Comment.Preproc */
.highlight .cpf { color: #408080; font-style: italic } /* Comment.PreprocFile */
.highlight .c1 { color: #408080; font-style: italic } /* Comment.Single */
.highlight .cs { color: #408080; font-style: italic } /* Comment.Special */
.highlight .gd { color: #A00000 } /* Generic.Deleted */
.highlight .ge { font-style: italic } /* Generic.Emph */
.highlight .gr { color: #FF0000 } /* Generic.Error */
.highlight .gh { color: #000080; font-weight: bold } /* Generic.Heading */
.highlight .gi { color: #00A000 } /* Generic.Inserted */
.highlight .go { color: #888888 } /* Generic.Output */
.highlight .gp { color: #000080; font-weight: bold } /* Generic.Prompt */
.highlight .gs { font-weight: bold } /* Generic.Strong */
.highlight .gu { color: #800080; font-weight: bold } /* Generic.Subheading */
.highlight .gt { color: #0044DD } /* Generic.Traceback */
.highlight .kc { color: #008000; font-weight: bold } /* Keyword.Constant */
.highlight .kd { color: #008000; font-weight: bold } /* Keyword.Declaration */
.highlight .kn { color: #008000; font-weight: bold } /* Keyword.Namespace */
.highlight .kp { color: #008000 } /* Keyword.Pseudo */
.highlight .kr { color: #008000; font-weight: bold } /* Keyword.Reserved */
.highlight .kt { color: #B00040 } /* Keyword.Type */
.highlight .m { color: #666666 } /* Literal.Number */
.highlight .s { color: #BA2121 } /* Literal.String */
.highlight .na { color: #7D9029 } /* Name.Attribute */
.highlight .nb { color: #008000 } /* Name.Builtin */
.highlight .nc { color: #0000FF; font-weight: bold } /* Name.Class */
.highlight .no { color: #880000 } /* Name.Constant */
.highlight .nd { color: #AA22FF } /* Name.Decorator */
.highlight .ni { color: #999999; font-weight: bold } /* Name.Entity */
.highlight .ne { color: #D2413A; font-weight: bold } /* Name.Exception */
.highlight .nf { color: #0000FF } /* Name.Function */
.highlight .nl { color: #A0A000 } /* Name.Label */
.highlight .nn { color: #0000FF; font-weight: bold } /* Name.Namespace */
.highlight .nt { color: #008000; font-weight: bold } /* Name.Tag */
.highlight .nv { color: #19177C } /* Name.Variable */
.highlight .ow { color: #AA22FF; font-weight: bold } /* Operator.Word */
.highlight .w { color: #bbbbbb } /* Text.Whitespace */
.highlight .mb { color: #666666 } /* Literal.Number.Bin */
.highlight .mf { color: #666666 } /* Literal.Number.Float */
.highlight .mh { color: #666666 } /* Literal.Number.Hex */
.highlight .mi { color: #666666 } /* Literal.Number.Integer */
.highlight .mo { color: #666666 } /* Literal.Number.Oct */
.highlight .sa { color: #BA2121 } /* Literal.String.Affix */
.highlight .sb { color: #BA2121 } /* Literal.String.Backtick */
.highlight .sc { color: #BA2121 } /* Literal.String.Char */
.highlight .dl { color: #BA2121 } /* Literal.String.Delimiter */
.highlight .sd { color: #BA2121; font-style: italic } /* Literal.String.Doc */
.highlight .s2 { color: #BA2121 } /* Literal.String.Double */
.highlight .se { color: #BB6622; font-weight: bold } /* Literal.String.Escape */
.highlight .sh { color: #BA2121 } /* Literal.String.Heredoc */
.highlight .si { color: #BB6688; font-weight: bold } /* Literal.String.Interpol */
.highlight .sx { color: #008000 } /* Literal.String.Other */
.highlight .sr { color: #BB6688 } /* Literal.String.Regex */
.highlight .s1 { color: #BA2121 } /* Literal.String.Single */
.highlight .ss { color: #19177C } /* Literal.String.Symbol */
.highlight .bp { color: #008000 } /* Name.Builtin.Pseudo */
.highlight .fm { color: #0000FF } /* Name.Function.Magic */
.highlight .vc { color: #19177C } /* Name.Variable.Class */
.highlight .vg { color: #19177C } /* Name.Variable.Global */
.highlight .vi { color: #19177C } /* Name.Variable.Instance */
.highlight .vm { color: #19177C } /* Name.Variable.Magic */
.highlight .il { color: #666666 } /* Literal.Number.Integer.Long */
    </style>


<style type="text/css">
/* Overrides of notebook CSS for static HTML export */
body {
  overflow: visible;
  padding: 8px;
}

div#notebook {
  overflow: visible;
  border-top: none;
}@media print {
  div.cell {
    display: block;
    page-break-inside: avoid;
  } 
  div.output_wrapper { 
    display: block;
    page-break-inside: avoid; 
  }
  div.output { 
    display: block;
    page-break-inside: avoid; 
  }
}
</style>

<!-- Custom stylesheet, it must be in the same directory as the html file -->
<link rel="stylesheet" href="custom.css">

<!-- Loading mathjax macro -->
<!-- Load mathjax -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/latest.js?config=TeX-AMS_HTML"></script>
    <!-- MathJax configuration -->
    <script type="text/x-mathjax-config">
    MathJax.Hub.Config({
        tex2jax: {
            inlineMath: [ ['$','$'], ["\\(","\\)"] ],
            displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
            processEscapes: true,
            processEnvironments: true
        },
        // Center justify equations in code and markdown cells. Elsewhere
        // we use CSS to left justify single line equations in code cells.
        displayAlign: 'center',
        "HTML-CSS": {
            styles: {'.MathJax_Display': {"margin": 0}},
            linebreaks: { automatic: true }
        }
    });
    </script>
    <!-- End of mathjax configuration --></head>
<body>
  <div tabindex="-1" id="notebook" class="border-box-sizing">
    <div class="container" id="notebook-container">

<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>For convenience, we can increase the display width of the Notebook to make better use of widescreen format</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[1]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">from</span> <span class="nn">IPython.core.display</span> <span class="kn">import</span> <span class="n">display</span><span class="p">,</span> <span class="n">HTML</span>
<span class="n">display</span><span class="p">(</span><span class="n">HTML</span><span class="p">(</span><span class="s2">&quot;&lt;style&gt;.container { width:90% !important; }&lt;/style&gt;&quot;</span><span class="p">))</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt"></div>



<div class="output_html rendered_html output_subarea ">
<style>.container { width:90% !important; }</style>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Next, we will import all the libraries that we need.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[2]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">import</span> <span class="nn">numpy</span> <span class="k">as</span> <span class="nn">np</span>
<span class="kn">import</span> <span class="nn">tensorflow</span> <span class="k">as</span> <span class="nn">tf</span>
<span class="kn">from</span> <span class="nn">tensorflow</span> <span class="kn">import</span> <span class="n">keras</span>
<span class="kn">from</span> <span class="nn">pathlib</span> <span class="kn">import</span> <span class="n">Path</span>
<span class="kn">from</span> <span class="nn">scipy.io</span> <span class="kn">import</span> <span class="n">wavfile</span>
<span class="kn">import</span> <span class="nn">python_speech_features</span>
<span class="kn">from</span> <span class="nn">tqdm.notebook</span> <span class="kn">import</span> <span class="n">tqdm</span>
<span class="kn">from</span> <span class="nn">sklearn.model_selection</span> <span class="kn">import</span> <span class="n">train_test_split</span>
<span class="kn">from</span> <span class="nn">sklearn.metrics</span> <span class="kn">import</span> <span class="n">confusion_matrix</span>
<span class="kn">from</span> <span class="nn">livelossplot</span> <span class="kn">import</span> <span class="n">PlotLossesKeras</span>
<span class="kn">import</span> <span class="nn">sounddevice</span> <span class="k">as</span> <span class="nn">sd</span>
<span class="o">%</span><span class="k">matplotlib</span> inline
<span class="kn">import</span> <span class="nn">matplotlib.pyplot</span> <span class="k">as</span> <span class="nn">plt</span>
<span class="kn">from</span> <span class="nn">datetime</span> <span class="kn">import</span> <span class="n">datetime</span>
<span class="kn">from</span> <span class="nn">scipy.signal</span> <span class="kn">import</span> <span class="n">butter</span><span class="p">,</span> <span class="n">sosfilt</span>
<span class="kn">from</span> <span class="nn">timeit</span> <span class="kn">import</span> <span class="n">default_timer</span> <span class="k">as</span> <span class="n">timer</span>
<span class="kn">from</span> <span class="nn">IPython.display</span> <span class="kn">import</span> <span class="n">clear_output</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>The Google Speech Command Dataset which we'll be using contains 30 different words, 20 core words and 10 auxiliary words. In this project we'll be using only the 20 core words.</p>
<p>We can define ourselves a dictionary that maps each different word to a number and a list that does the inverse mapping. This is necessary because we need numerical class labels for our Neural Network.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[3]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">word2index</span> <span class="o">=</span> <span class="p">{</span>
    <span class="c1"># core words</span>
    <span class="s2">&quot;yes&quot;</span><span class="p">:</span> <span class="mi">0</span><span class="p">,</span>
    <span class="s2">&quot;no&quot;</span><span class="p">:</span> <span class="mi">1</span><span class="p">,</span>
    <span class="s2">&quot;up&quot;</span><span class="p">:</span> <span class="mi">2</span><span class="p">,</span>
    <span class="s2">&quot;down&quot;</span><span class="p">:</span> <span class="mi">3</span><span class="p">,</span>
    <span class="s2">&quot;left&quot;</span><span class="p">:</span> <span class="mi">4</span><span class="p">,</span>
    <span class="s2">&quot;right&quot;</span><span class="p">:</span> <span class="mi">5</span><span class="p">,</span>
    <span class="s2">&quot;on&quot;</span><span class="p">:</span> <span class="mi">6</span><span class="p">,</span>
    <span class="s2">&quot;off&quot;</span><span class="p">:</span> <span class="mi">7</span><span class="p">,</span>
    <span class="s2">&quot;stop&quot;</span><span class="p">:</span> <span class="mi">8</span><span class="p">,</span>
    <span class="s2">&quot;go&quot;</span><span class="p">:</span> <span class="mi">9</span><span class="p">,</span>
    <span class="s2">&quot;zero&quot;</span><span class="p">:</span> <span class="mi">10</span><span class="p">,</span>
    <span class="s2">&quot;one&quot;</span><span class="p">:</span> <span class="mi">11</span><span class="p">,</span>
    <span class="s2">&quot;two&quot;</span><span class="p">:</span> <span class="mi">12</span><span class="p">,</span>
    <span class="s2">&quot;three&quot;</span><span class="p">:</span> <span class="mi">13</span><span class="p">,</span>
    <span class="s2">&quot;four&quot;</span><span class="p">:</span> <span class="mi">14</span><span class="p">,</span>
    <span class="s2">&quot;five&quot;</span><span class="p">:</span> <span class="mi">15</span><span class="p">,</span>
    <span class="s2">&quot;six&quot;</span><span class="p">:</span> <span class="mi">16</span><span class="p">,</span>
    <span class="s2">&quot;seven&quot;</span><span class="p">:</span> <span class="mi">17</span><span class="p">,</span>
    <span class="s2">&quot;eight&quot;</span><span class="p">:</span> <span class="mi">18</span><span class="p">,</span>
    <span class="s2">&quot;nine&quot;</span><span class="p">:</span> <span class="mi">19</span><span class="p">,</span>
<span class="p">}</span>

<span class="n">index2word</span> <span class="o">=</span> <span class="p">[</span><span class="n">word</span> <span class="k">for</span> <span class="n">word</span> <span class="ow">in</span> <span class="n">word2index</span><span class="p">]</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Next, we will go trough the dataset and save all the paths to the data samples in a list.</p>
<p>You can download the Google Speech Commands dataset <a href="http://download.tensorflow.org/data/speech_commands_v0.01.tar.gz">here</a> (1.4 GB!)
and decompress it (e.g. with 7zip). Define the path where you stored the decommpressed dataset as <em>speech_commands_dataset_basepath</em>.</p>
<p>The dataset doesn't contain an equal number of samples for each word, but it contains &gt;2000 valid samples for each of the 20 core words.
Each sample is supposed to  be exactly 1s long, which at a sampling rate of 16kHz and 16bit quantization should result in 32044 Byte large files.
Somehow some samples in the dataset are not exactly that size, we skip those.</p>
<p>Additionally we'll use a nice <a href="https://tqdm.github.io/">tqdm</a> progress bar to make it more fancy. In the end, we should have gathered a total of 40000 samples.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[17]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">num_classes</span> <span class="o">=</span> <span class="nb">len</span><span class="p">(</span><span class="n">word2index</span><span class="p">)</span>
<span class="n">num_samples_per_class</span> <span class="o">=</span> <span class="mi">2000</span>
<span class="n">speech_commands_dataset_basepath</span> <span class="o">=</span> <span class="n">Path</span><span class="p">(</span><span class="sa">r</span><span class="s2">&quot;C:\Users\tunin\Desktop\Marcel_Python\speech_command_dataset&quot;</span><span class="p">)</span>

<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;loading dataset...&quot;</span><span class="p">)</span>
<span class="n">samples</span> <span class="o">=</span> <span class="p">[]</span>
<span class="n">classes</span> <span class="o">=</span>  <span class="p">[]</span>
<span class="k">with</span> <span class="n">tqdm</span><span class="p">(</span><span class="n">total</span><span class="o">=</span><span class="n">num_samples_per_class</span><span class="o">*</span><span class="mi">20</span><span class="p">)</span> <span class="k">as</span> <span class="n">pbar</span><span class="p">:</span>
    <span class="k">for</span> <span class="n">word_class</span> <span class="ow">in</span> <span class="n">word2index</span><span class="p">:</span>
        <span class="n">folder</span> <span class="o">=</span> <span class="n">speech_commands_dataset_basepath</span> <span class="o">/</span> <span class="n">word_class</span> <span class="c1"># sub-folder for each word</span>
        <span class="n">count</span> <span class="o">=</span> <span class="mi">0</span>
        <span class="k">for</span> <span class="n">file</span> <span class="ow">in</span> <span class="n">folder</span><span class="o">.</span><span class="n">iterdir</span><span class="p">():</span> <span class="c1"># iterate over all files in the folder</span>
            <span class="c1"># somehow, there are samples which aren&#39;t exactly 1 s long in the dataset. ignore those</span>
            <span class="k">if</span> <span class="n">file</span><span class="o">.</span><span class="n">stat</span><span class="p">()</span><span class="o">.</span><span class="n">st_size</span> <span class="o">==</span> <span class="mi">32044</span><span class="p">:</span>
                <span class="n">samples</span><span class="o">.</span><span class="n">append</span><span class="p">(</span><span class="n">file</span><span class="p">)</span> <span class="c1"># store path of sample file</span>
                <span class="n">classes</span><span class="o">.</span><span class="n">append</span><span class="p">(</span><span class="n">word2index</span><span class="p">[</span><span class="n">word_class</span><span class="p">])</span> <span class="c1"># append word class index to list</span>
                <span class="n">count</span> <span class="o">+=</span><span class="mi">1</span>
                <span class="n">pbar</span><span class="o">.</span><span class="n">update</span><span class="p">()</span>
            <span class="k">if</span> <span class="n">count</span> <span class="o">&gt;=</span> <span class="n">num_samples_per_class</span><span class="p">:</span>
                <span class="k">break</span>
<span class="n">classes</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">array</span><span class="p">(</span><span class="n">classes</span><span class="p">,</span> <span class="n">dtype</span><span class="o">=</span><span class="n">np</span><span class="o">.</span><span class="n">int</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt"></div>


<div class="output_subarea output_stream output_stdout output_text">
<pre>loading dataset...
</pre>
</div>
</div>

<div class="output_area">

    <div class="prompt"></div>





 
 
<div id="3bcbefd8-2b55-4eb5-9092-2a54c0a7d75f"></div>
<div class="output_subarea output_widget_view ">
<script type="text/javascript">
var element = $('#3bcbefd8-2b55-4eb5-9092-2a54c0a7d75f');
</script>
<script type="application/vnd.jupyter.widget-view+json">
{"model_id": "9213523856e14c608824973685d978c5", "version_major": 2, "version_minor": 0}
</script>
</div>

</div>

<div class="output_area">

    <div class="prompt"></div>


<div class="output_subarea output_stream output_stdout output_text">
<pre>
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Next we'll define two functions to compute some <a href="https://en.wikipedia.org/wiki/Feature_(machine_learning">features</a>) of the audio samples and another function which loads the sample wav-file and then computes the features. Before computing features, it is a good idea to normalize our input data. Depending on your recording device and other parameters, the amplitudes of the recorded audio signals may vary drastically and might even have an offset. Thus we can subtract the mean value to remove the offset and divide by the absolute maximum value of the signal, so that it's new range lies between -1.0 and +1.0.</p>
<p>In this case, we'll use the so called "Mel-Frequency Cepstrum Coefficients", which are very commonly used for speech recognition tasks. The MFCCs are computed as follows:</p>
<ul>
<li><p>Apply a simple pre-emphasis filter to the signal, to emphasize higher frequencies (optional):</p>
<p>$y_t \leftarrow y_t - \alpha \cdot y_{t-1} \hspace{1mm} , \hspace{2mm} t=1...T$</p>
</li>
</ul>
<ul>
<li>Extract snippets from the audio signal. A good choice for the length of the snippets is 25ms. The stride between snippets is 10ms, so the snippets will overlap. To "cut" the snippets from the audio signal, a window like the Hamming window is appropriate to mitigate the leakage effect when performing the Fourier Transform in the next step.</li>
</ul>
<ul>
<li>Calculate the FFT of the signal and then the power spectrum, i.e. the squared magnitude of the spectrum for each snippet.</li>
</ul>
<ul>
<li><p>Apply a Mel filterbank to the power spectrum of each snippet. The <a href="https://en.wikipedia.org/wiki/Mel_scale">Mel scale</a> is a scale, that takes into account the fact, that in human auditory perception, the perceivable pitch (i.e. frequency) changes decrease with higher frequencies. This means e.g. that we can distinguish much better between a 200Hz and 300Hz tone than between a 10200 Hz and a 10300 Hz tone even though the absolute difference is the same.</p>
<p>The filterbank consists of $N=40$ triangular filters evenly spaced in the Mel scale (and nonlinearly spaced in frequency scale). These filters are multiplied with the power spectrum, which gives us the sum of "energies" in each filter. Additionally, we take the log() of these energies.</p>
</li>
</ul>
<ul>
<li>The log-energies of adjacent filters usually correlate strongly. Therefore, a <a href="https://en.wikipedia.org/wiki/Discrete_cosine_transform">Discrete Cosine Transform</a> is applied to the log filterbank energies of each snippet. The resulting values are called <em>Cepstral coefficients</em>. The zeroth coefficient represents the average log-energy in each snippet, it may or may not be discarded (here we'll keep it as a feature). Usually, only a subset, e.g. the first 8-12 Cepstral coefficients are used (here we'll use 20), the rest are discarded </li>
</ul>
<p>For more details about MFCC, a good source is:<br>
<em>pp. 85-72 in K.S. Rao and Manjunath K.E., "Speech Recognition Using Articulatory and Excitation Source Features", 2017, Springer</em><br>
(pp. 85-92 available for preview at [<a href="https://link.springer.com/content/pdf/bbm%3A978-3-319-49220-9%2F1.pdf">https://link.springer.com/content/pdf/bbm%3A978-3-319-49220-9%2F1.pdf</a>])</p>
<p>Thankfully we don't have to implement the MFCC computation ourselves, we'll use the library <a href="https://python-speech-features.readthedocs.io/en/latest/">python_speech_features</a>.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[4]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># compute MFCC features from audio signal</span>
<span class="k">def</span> <span class="nf">audio2feature</span><span class="p">(</span><span class="n">audio</span><span class="p">):</span>
    <span class="n">audio</span> <span class="o">=</span> <span class="n">audio</span><span class="o">.</span><span class="n">astype</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">float</span><span class="p">)</span>
    <span class="c1"># normalize data</span>
    <span class="n">audio</span> <span class="o">-=</span> <span class="n">audio</span><span class="o">.</span><span class="n">mean</span><span class="p">()</span>
    <span class="n">audio</span> <span class="o">/=</span> <span class="n">np</span><span class="o">.</span><span class="n">max</span><span class="p">((</span><span class="n">audio</span><span class="o">.</span><span class="n">max</span><span class="p">(),</span> <span class="o">-</span><span class="n">audio</span><span class="o">.</span><span class="n">min</span><span class="p">()))</span>
    <span class="c1"># compute MFCC coefficients</span>
    <span class="n">features</span> <span class="o">=</span> <span class="n">python_speech_features</span><span class="o">.</span><span class="n">mfcc</span><span class="p">(</span><span class="n">audio</span><span class="p">,</span> <span class="n">samplerate</span><span class="o">=</span><span class="mi">16000</span><span class="p">,</span> <span class="n">winlen</span><span class="o">=</span><span class="mf">0.025</span><span class="p">,</span> <span class="n">winstep</span><span class="o">=</span><span class="mf">0.01</span><span class="p">,</span> <span class="n">numcep</span><span class="o">=</span><span class="mi">20</span><span class="p">,</span> <span class="n">nfilt</span><span class="o">=</span><span class="mi">40</span><span class="p">,</span> <span class="n">nfft</span><span class="o">=</span><span class="mi">512</span><span class="p">,</span> <span class="n">lowfreq</span><span class="o">=</span><span class="mi">100</span><span class="p">,</span> <span class="n">highfreq</span><span class="o">=</span><span class="kc">None</span><span class="p">,</span> <span class="n">preemph</span><span class="o">=</span><span class="mf">0.97</span><span class="p">,</span> <span class="n">ceplifter</span><span class="o">=</span><span class="mi">22</span><span class="p">,</span> <span class="n">appendEnergy</span><span class="o">=</span><span class="kc">True</span><span class="p">,</span> <span class="n">winfunc</span><span class="o">=</span><span class="n">np</span><span class="o">.</span><span class="n">hamming</span><span class="p">)</span>
    <span class="k">return</span> <span class="n">features</span>

<span class="c1"># load .wav-file, add some noise and compute MFCC features</span>
<span class="k">def</span> <span class="nf">wav2feature</span><span class="p">(</span><span class="n">filepath</span><span class="p">):</span>
    <span class="n">samplerate</span><span class="p">,</span> <span class="n">data</span> <span class="o">=</span> <span class="n">wavfile</span><span class="o">.</span><span class="n">read</span><span class="p">(</span><span class="n">filepath</span><span class="p">)</span>
    <span class="n">data</span> <span class="o">=</span> <span class="n">data</span><span class="o">.</span><span class="n">astype</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">float</span><span class="p">)</span>
    <span class="c1"># normalize data</span>
    <span class="n">data</span> <span class="o">-=</span> <span class="n">data</span><span class="o">.</span><span class="n">mean</span><span class="p">()</span>
    <span class="n">data</span> <span class="o">/=</span> <span class="n">np</span><span class="o">.</span><span class="n">max</span><span class="p">((</span><span class="n">data</span><span class="o">.</span><span class="n">max</span><span class="p">(),</span> <span class="o">-</span><span class="n">data</span><span class="o">.</span><span class="n">min</span><span class="p">()))</span>
    <span class="c1"># add gaussian noise</span>
    <span class="n">data</span> <span class="o">+=</span> <span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">normal</span><span class="p">(</span><span class="n">loc</span><span class="o">=</span><span class="mf">0.0</span><span class="p">,</span> <span class="n">scale</span><span class="o">=</span><span class="mf">0.025</span><span class="p">,</span> <span class="n">size</span><span class="o">=</span><span class="n">data</span><span class="o">.</span><span class="n">shape</span><span class="p">)</span>
    <span class="c1"># compute MFCC coefficients</span>
    <span class="n">features</span> <span class="o">=</span> <span class="n">python_speech_features</span><span class="o">.</span><span class="n">mfcc</span><span class="p">(</span><span class="n">data</span><span class="p">,</span> <span class="n">samplerate</span><span class="o">=</span><span class="mi">16000</span><span class="p">,</span> <span class="n">winlen</span><span class="o">=</span><span class="mf">0.025</span><span class="p">,</span> <span class="n">winstep</span><span class="o">=</span><span class="mf">0.01</span><span class="p">,</span> <span class="n">numcep</span><span class="o">=</span><span class="mi">20</span><span class="p">,</span> <span class="n">nfilt</span><span class="o">=</span><span class="mi">40</span><span class="p">,</span> <span class="n">nfft</span><span class="o">=</span><span class="mi">512</span><span class="p">,</span> <span class="n">lowfreq</span><span class="o">=</span><span class="mi">100</span><span class="p">,</span> <span class="n">highfreq</span><span class="o">=</span><span class="kc">None</span><span class="p">,</span> <span class="n">preemph</span><span class="o">=</span><span class="mf">0.97</span><span class="p">,</span> <span class="n">ceplifter</span><span class="o">=</span><span class="mi">22</span><span class="p">,</span> <span class="n">appendEnergy</span><span class="o">=</span><span class="kc">True</span><span class="p">,</span> <span class="n">winfunc</span><span class="o">=</span><span class="n">np</span><span class="o">.</span><span class="n">hamming</span><span class="p">)</span>
    <span class="k">return</span> <span class="n">features</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>If we compute the features for one audio sample, we see that the feature shape is (99, 20). The first index is that of the 10ms long snippet of the 1s long audio signal, so we have 1s/10ms-1=99 snippets. The second dimension is the number of MFC coefficients, in this case we have 20.</p>
<p>Now we can load all audio samples and pre-compute the MFCC features for each sample. Note that this will take quite a long time!</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[18]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">feature_shape</span> <span class="o">=</span> <span class="n">wav2feature</span><span class="p">(</span><span class="n">samples</span><span class="p">[</span><span class="mi">0</span><span class="p">])</span><span class="o">.</span><span class="n">shape</span>
<span class="n">features</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">empty</span><span class="p">((</span><span class="n">num_classes</span><span class="o">*</span><span class="n">num_samples_per_class</span><span class="p">,</span> <span class="p">)</span><span class="o">+</span><span class="p">(</span><span class="n">feature_shape</span><span class="p">),</span> <span class="n">dtype</span><span class="o">=</span><span class="n">np</span><span class="o">.</span><span class="n">float</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;features.shape&quot;</span><span class="p">,</span> <span class="n">features</span><span class="o">.</span><span class="n">shape</span><span class="p">)</span>

<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;pre-computing features from audio files...&quot;</span><span class="p">)</span>
<span class="k">with</span> <span class="n">tqdm</span><span class="p">(</span><span class="n">total</span><span class="o">=</span><span class="n">num_samples_per_class</span><span class="o">*</span><span class="n">num_classes</span><span class="p">)</span> <span class="k">as</span> <span class="n">pbar</span><span class="p">:</span>
    <span class="k">for</span> <span class="n">k</span><span class="p">,</span> <span class="n">sample</span> <span class="ow">in</span> <span class="nb">enumerate</span><span class="p">(</span><span class="n">samples</span><span class="p">):</span>
        <span class="n">features</span><span class="p">[</span><span class="n">k</span><span class="p">]</span> <span class="o">=</span> <span class="n">wav2feature</span><span class="p">(</span><span class="n">sample</span><span class="p">)</span>
        <span class="n">pbar</span><span class="o">.</span><span class="n">update</span><span class="p">()</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt"></div>


<div class="output_subarea output_stream output_stdout output_text">
<pre>features.shape (40000, 99, 20)
pre-computing features from audio files...
</pre>
</div>
</div>

<div class="output_area">

    <div class="prompt"></div>





 
 
<div id="69ddd6e2-cfc7-49bb-808a-b1be85bb8671"></div>
<div class="output_subarea output_widget_view ">
<script type="text/javascript">
var element = $('#69ddd6e2-cfc7-49bb-808a-b1be85bb8671');
</script>
<script type="application/vnd.jupyter.widget-view+json">
{"model_id": "e1fcaf5cc38b47488d3086060ec756cc", "version_major": 2, "version_minor": 0}
</script>
</div>

</div>

<div class="output_area">

    <div class="prompt"></div>


<div class="output_subarea output_stream output_stdout output_text">
<pre>
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Now we can save the pre-computed training dataset containing the features of the training samples and their class labels.<br>
This way, we won't have to re-compute the features next time.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[23]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># save computed features and classes to hard drive</span>
<span class="n">np</span><span class="o">.</span><span class="n">save</span><span class="p">(</span><span class="s2">&quot;mfcc_plus_energy_features_40000x99x20&quot;</span><span class="p">,</span> <span class="n">features</span><span class="p">)</span>
<span class="n">np</span><span class="o">.</span><span class="n">save</span><span class="p">(</span><span class="s2">&quot;classes&quot;</span><span class="p">,</span> <span class="n">np</span><span class="o">.</span><span class="n">array</span><span class="p">(</span><span class="n">classes</span><span class="p">,</span> <span class="n">dtype</span><span class="o">=</span><span class="n">np</span><span class="o">.</span><span class="n">int</span><span class="p">))</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>We can load the pre-computed features and class labels as follows:</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[5]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># load pre-computed training features dataset and training class labels</span>
<span class="n">features</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">load</span><span class="p">(</span><span class="s2">&quot;mfcc_plus_energy_features_40000x99x20.npy&quot;</span><span class="p">)</span>
<span class="n">classes</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">load</span><span class="p">(</span><span class="s2">&quot;classes.npy&quot;</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Now the next thing to do is divide our dataset into a training dataset and a validation dataset.<br>
The training dataset is used for training our Neural Network, i.e. the Neural Network will learn to correctly predict a sample's class label based on it's features.</p>
<p>One problem that can occur in Machine Learning is so called <em>Overfitting</em>. Our basic goal is to train our Neural Network so that it does not only classify the training samples correctly, but also new samples, which it has never "seen" before. This is called <em>Generalization</em>. But with complex networks it can happen that instead of really learning to classify samples based on their features, the network simply "learns by heart" to which class each training sample belongs. This is called Overfitting. In this case, the network will perform great on the training data, but poorly on new previously unseen data.</p>
<p>One method to mitigate, is the use of a separate validation dataset. So we split the whole dataset, and use a small subset (e.g. here one third of the data) for validation, the rest is our training set.<br>
Now during training, only the training dataset will be used to calculate the weigths of the Neural Network (which is the "learning" part). After each epoch (i.e. once all training samples have been considered once), we will tell the network to try and predict the class labels of all samples in our validation dataset and based on that, calculate the accuracy on the validation set.</p>
<p>So during training, after each training epoch, we can look at the accuracy of the network on the training set and on the validation set.<br>
At the beginning of the training, both accuracies will typically improve. At one point we might see that the validation accuracy plateaus or evendecreases, while the training accuracy still improves. This indicates that the network is starting to overfit, thus it is a good time to stop the training.</p>
<p>Another method to mitigate overfitting is the use of so called <a href="https://en.wikipedia.org/wiki/Dilution_(neural_networks">Dropout-Layers</a>) which randomly set a subset of the weigths of a layer to zero. In this project, we won't use them.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[8]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">train_data</span><span class="p">,</span> <span class="n">validation_data</span><span class="p">,</span> <span class="n">train_classes</span><span class="p">,</span> <span class="n">validation_classes</span> <span class="o">=</span> <span class="n">train_test_split</span><span class="p">(</span><span class="n">features</span><span class="p">,</span> <span class="n">classes</span><span class="p">,</span>
                                                                      <span class="n">test_size</span><span class="o">=</span><span class="mf">0.30</span><span class="p">,</span> <span class="n">random_state</span><span class="o">=</span><span class="mi">42</span><span class="p">,</span> <span class="n">shuffle</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>The next step is to define our Neural Network's architecture. The network can be described by a sequence of layers.</p>
<p>For this task we will implement a <a href="https://en.wikipedia.org/wiki/Convolutional_neural_network">Convolutional Neural Network (CNN)</a>. The two main characteristics of CNNs are convolutional layers and pooling layers.</p>
<p>Convolutional layers convolve a filter vector (1D) or matrix (2D) with the input data. The main advantage of convolutional layers (and thus of CNNs) is, that they can achieve a high degree of shift-/translation-invariance. Picture the following example: we have two 2s long recordings of the same spoken word, but in one recording the word is right at the beginning of the recording, and in the other one at the end. Now a conventional Neural Network might have a hard time learning to recognize the words, because it expects certain features at certain position in time. Another example migth be an image classifier that recognizes objects well if they're all in the center of the image and in the same orientation, but fails if the objects are in a corner of the image or rotated. So we want the network to be invariant to translations and rotations of the features, i.e. recognize features regardless of their position in time (e.g. in case of audio) or space (e.g. in case of an image).</p>
<p>A convolutional layer needs 3 parameters:</p>
<ul>
<li>filter size $F$: width (1D) or height and width (2D) of filter vector/matrix. Determines number of weigths of the layer.</li>
<li>stride $S$: determines the step size with which we move the filter across the signal or image</li>
<li>padding $P$: pad the input data with zeros<br>
The size of the convolutional layer's output is:<br>
# $W_{out}=\frac{W_{in}-F-2P}{S}+1$  </li>
</ul>
<p>In Keras, the default stride is one and the default padding is zero.</p>
<p>A pooling layer is somewhat similar in that it also convolves a "filter"-vector/matrix across the input with a certain stride and possibly padding. But instead of multiplying the input values with the filter values, the pooling layer computes either the average or maximum value of the values. Max-pooling layers are commonly used, average pooling rarely. So e.g. a 3x3 max-pooling layer will slide a 3x3-filter over the input and deliver the maximum of the 3*3=9 values as output. In contrary to the convolutional layer, a pooling layer introduces no additional weights. The output size of a pooling layer can be calculated with the same formula as for the convolutional layer.<br>
In Keras, the default stride is equal to the filter size and the default padding is zero. In this case the formula simplifies to:</p>
<h1 id="$W_{out}=\frac{W_{in}-F-2P}{S}+1=\frac{W_{in}-F}{F}+1=\frac{W_{in}}{F}$">$W_{out}=\frac{W_{in}-F-2P}{S}+1=\frac{W_{in}-F}{F}+1=\frac{W_{in}}{F}$<a class="anchor-link" href="#$W_{out}=\frac{W_{in}-F-2P}{S}+1=\frac{W_{in}-F}{F}+1=\frac{W_{in}}{F}$">&#182;</a></h1><p>A max-pooling layer achieves a down-sampling of the feature vector/matrix and also a translation/shift-invariance of the features. It is common to use a pooling layer after a convolution layer.</p>
<p>We'll be creating our CNN model using keras' <a href="https://keras.io/guides/sequential_model/">Sequential</a> model.<br>
At first, we add an input layer whose input size matches the dimensions of our MFCC features. In this case wee have 20 MFC coefficients and 99 timeframes, thus the feature matrix for an audio sample is of size (99, 20). In Keras, the input shapes are by default as follows:</p>
<ul>
<li>(batch, axis, channel): one-dimensional data with $\geq$1 channels. This most commonly represents a timeseries, i.e. a number of features that change over time.<br>
In our case, the (99, 20) feature matrix is interpreted as a time series (i.e. axis represents time(-frame)) with 20 channels (the MFC coefficients. We can perform a 1D-convolution on such data.</li>
<li>(batch, axis0, axis1, channel): two-dimensional data with $\geq$1 channels. This is most often a color image, where axis0 and axis1 are the horizontal and vertical position of an image pixel and each pixel has 3 channels for the red, green and blue color values. We can perform a 2D-convolution on such data.</li>
<li>(batch, axis0, axis1, ..., axisN-1, channel): n-dimensional data with $\geq$1 channels.</li>
</ul>
<p>Now you may wonder about the batch dimension. This is another dimension that specifies the number of samples, because during training we often load batches of more than one sample in one iteration to average the gradients during optimization. In Keras, during model specification the batch dimension is ignored, so we won't have to specify it explicitly. But as you can see, our <em>features</em> variable, which contains all training samples has the shape (40000, 99, 20), so its first axis is the batch dimension. This way when we'll later pass the training data to the <em>fit()</em>-function, it can fetch a batch, i.e. a subset of the dataset for each training iteration.</p>
<p>Next, we add a 1-D convolutional layer (<a href="https://keras.io/api/layers/convolution_layers/convolution1d/">Conv1D</a>). This layer performs a one dimensional convolution along the (in our case) time (or more precisely timeframe) axis. The first argument is the number of filters to apply. Most often we use many filters, thus performing the convolution multiple times with different filter kernels. This way, the number of channels of the convolutional layer's output is the number of filters used. The second argument is the kernel size, this is the size of our convolution filter. At last, we specify an activation function used, in this case the ReLU-function is used.<br>
After the first convolutional layer, we add a max pooling layer with a size of 3 that reduces the data size along the time axis. Note that in case the division is fractional, the resulting size will be the floor value.</p>
<p>Such a combination of convolutional layer and pooling layer is very common in CNNs. The main idea is to repeatedly stack convolution and pooling layers, so that the dimension (in time or space) of the input data is subsequently reduced, while the feature space dimensionality (i.e. number of channels) increases.</p>
<p>Next, we add two more stacks of convolutional and max pooling layer. For the last pooling layer, we use a global pooling layer, which behaves just like the normal pooling layer, but with a filter that spans the whole axis size. After the global max pooling operation, our data is one-dimensional, with the time axis completely removed and only the feature dimension remaining.</p>
<p>In the next step, we add a couple of fully connected (Keras calles them "dense") layers, just like in a regular Multi Layer Perceptron (MLP). Each layer reduces the feature dimensionality, so that the last layer has an output dimension equal to the number of different classes (in our case words). Using the Softmax activation function on the last dense layer, we can interpret the networks output as an a posteriori probability distribution of the sample belonging to a certain class, given the audio sample's input features.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[25]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">keras</span><span class="o">.</span><span class="n">backend</span><span class="o">.</span><span class="n">clear_session</span><span class="p">()</span> <span class="c1"># clear previous model (if cell is executed more than once)</span>

<span class="c1">### CNN MODEL DEFINITION ###</span>
<span class="n">model</span> <span class="o">=</span> <span class="n">keras</span><span class="o">.</span><span class="n">models</span><span class="o">.</span><span class="n">Sequential</span><span class="p">()</span>

<span class="n">model</span><span class="o">.</span><span class="n">add</span><span class="p">(</span><span class="n">keras</span><span class="o">.</span><span class="n">layers</span><span class="o">.</span><span class="n">Input</span><span class="p">(</span><span class="n">shape</span><span class="o">=</span><span class="p">(</span><span class="mi">99</span><span class="p">,</span> <span class="mi">20</span><span class="p">)))</span>

<span class="n">model</span><span class="o">.</span><span class="n">add</span><span class="p">(</span><span class="n">keras</span><span class="o">.</span><span class="n">layers</span><span class="o">.</span><span class="n">Conv1D</span><span class="p">(</span><span class="mi">64</span><span class="p">,</span> <span class="n">kernel_size</span><span class="o">=</span><span class="mi">8</span><span class="p">,</span> <span class="n">activation</span><span class="o">=</span><span class="s2">&quot;relu&quot;</span><span class="p">))</span>
<span class="n">model</span><span class="o">.</span><span class="n">add</span><span class="p">(</span><span class="n">keras</span><span class="o">.</span><span class="n">layers</span><span class="o">.</span><span class="n">MaxPooling1D</span><span class="p">(</span><span class="n">pool_size</span><span class="o">=</span><span class="mi">3</span><span class="p">))</span>

<span class="n">model</span><span class="o">.</span><span class="n">add</span><span class="p">(</span><span class="n">keras</span><span class="o">.</span><span class="n">layers</span><span class="o">.</span><span class="n">Conv1D</span><span class="p">(</span><span class="mi">128</span><span class="p">,</span> <span class="n">kernel_size</span><span class="o">=</span><span class="mi">8</span><span class="p">,</span> <span class="n">activation</span><span class="o">=</span><span class="s2">&quot;relu&quot;</span><span class="p">))</span>
<span class="n">model</span><span class="o">.</span><span class="n">add</span><span class="p">(</span><span class="n">keras</span><span class="o">.</span><span class="n">layers</span><span class="o">.</span><span class="n">MaxPooling1D</span><span class="p">(</span><span class="n">pool_size</span><span class="o">=</span><span class="mi">3</span><span class="p">))</span>

<span class="n">model</span><span class="o">.</span><span class="n">add</span><span class="p">(</span><span class="n">keras</span><span class="o">.</span><span class="n">layers</span><span class="o">.</span><span class="n">Conv1D</span><span class="p">(</span><span class="mi">256</span><span class="p">,</span> <span class="n">kernel_size</span><span class="o">=</span><span class="mi">5</span><span class="p">,</span> <span class="n">activation</span><span class="o">=</span><span class="s2">&quot;relu&quot;</span><span class="p">))</span>
<span class="n">model</span><span class="o">.</span><span class="n">add</span><span class="p">(</span><span class="n">keras</span><span class="o">.</span><span class="n">layers</span><span class="o">.</span><span class="n">GlobalMaxPooling1D</span><span class="p">())</span>

<span class="n">model</span><span class="o">.</span><span class="n">add</span><span class="p">(</span><span class="n">keras</span><span class="o">.</span><span class="n">layers</span><span class="o">.</span><span class="n">Dense</span><span class="p">(</span><span class="mi">128</span><span class="p">,</span> <span class="n">activation</span><span class="o">=</span><span class="s2">&quot;relu&quot;</span><span class="p">))</span>

<span class="n">model</span><span class="o">.</span><span class="n">add</span><span class="p">(</span><span class="n">keras</span><span class="o">.</span><span class="n">layers</span><span class="o">.</span><span class="n">Dense</span><span class="p">(</span><span class="mi">64</span><span class="p">,</span> <span class="n">activation</span><span class="o">=</span><span class="s2">&quot;relu&quot;</span><span class="p">))</span>

<span class="n">model</span><span class="o">.</span><span class="n">add</span><span class="p">(</span><span class="n">keras</span><span class="o">.</span><span class="n">layers</span><span class="o">.</span><span class="n">Dense</span><span class="p">(</span><span class="n">num_classes</span><span class="p">,</span> <span class="n">activation</span><span class="o">=</span><span class="s1">&#39;softmax&#39;</span><span class="p">))</span>

<span class="c1"># print model architecture</span>
<span class="n">model</span><span class="o">.</span><span class="n">summary</span><span class="p">()</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt"></div>


<div class="output_subarea output_stream output_stdout output_text">
<pre>Model: &#34;sequential&#34;
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
conv1d (Conv1D)              (None, 92, 64)            10304     
_________________________________________________________________
max_pooling1d (MaxPooling1D) (None, 30, 64)            0         
_________________________________________________________________
conv1d_1 (Conv1D)            (None, 23, 128)           65664     
_________________________________________________________________
max_pooling1d_1 (MaxPooling1 (None, 7, 128)            0         
_________________________________________________________________
conv1d_2 (Conv1D)            (None, 3, 256)            164096    
_________________________________________________________________
global_max_pooling1d (Global (None, 256)               0         
_________________________________________________________________
dense (Dense)                (None, 128)               32896     
_________________________________________________________________
dense_1 (Dense)              (None, 64)                8256      
_________________________________________________________________
dense_2 (Dense)              (None, 20)                1300      
=================================================================
Total params: 282,516
Trainable params: 282,516
Non-trainable params: 0
_________________________________________________________________
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Now that our CNN model is defined, we can configure it for training. Therefore we choose an optimization algorithm, e.g. Stochastic Gradient Descent (SGD) or ADAM. Additionally, we need to specify a loss function for training. The loss function determines, how the performance of the network is evaluated. In this case, we have a multi-class classification problem, where the class labels are represented as integer values. In this case, the sparse categorical cross-entropy loss can be used. If our class labels were encoded using a one-hot encoding scheme, we would use the normal (non-sparse) variant. As a metric we specify the accuracy so that after every epoch, the accuracy of the network is computed.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[26]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">sgd</span> <span class="o">=</span> <span class="n">keras</span><span class="o">.</span><span class="n">optimizers</span><span class="o">.</span><span class="n">SGD</span><span class="p">()</span>
<span class="n">loss_fn</span> <span class="o">=</span> <span class="n">keras</span><span class="o">.</span><span class="n">losses</span><span class="o">.</span><span class="n">SparseCategoricalCrossentropy</span><span class="p">()</span> <span class="c1"># use Sparse because classes are represented as integers not as one-hot encoding</span>

<span class="n">model</span><span class="o">.</span><span class="n">compile</span><span class="p">(</span><span class="n">optimizer</span><span class="o">=</span><span class="n">sgd</span><span class="p">,</span> <span class="n">loss</span><span class="o">=</span><span class="n">loss_fn</span><span class="p">,</span> <span class="n">metrics</span><span class="o">=</span><span class="p">[</span><span class="s2">&quot;accuracy&quot;</span><span class="p">])</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Before starting the training, it can be useful to define an early stopping criterion in order to avoid overfitting as explained previously. We define a callback function which checks if the accuracy on the validation set has increased in the last 5 epochs and stops training if this is not the case. After stopping, the model is reverted to the state (i.e. the weigths) which had achieved the best result.</p>
<p>We'll also use the <a href="https://github.com/stared/livelossplot">livelossplot</a> library, which provides functions to plot a live graph of the accuracy and loss metrics during training. We pass the plot function as a callback too.</p>
<p>Finally, we can start training using <em>model.fit()</em>. We specify the training and validation dataset, the max number of epochs to train, the callbacks and the batch size. In this case, a batch size of 32 is used, so that in every iteration, a batch of 32 samples is used to compute the gradient. Especially when using SGD, the batch size influences the training. In each iteration, the average of the gradients for the batch is computed and used to update the weights. A smaller batch leads to a "noisy" gradient which can be good to explore the weigth space further (and not get stuck very early in  a local minimum), but affects convergence towards the end of training negatively. A larger batch leads to less noisy gradients, so that larger steps can be made (i.e. a higher learning-rate) which lead to faster training. Additionally, larger batches tend to reduce computation overhead. A batch size of one would be "pure" stochastic gradient descent, while a batch equal to the whole training set would be considered standard (i.e. non-stochastic) gradient descent. With a batch size in between (often called "mini batch"), a good compromise can be found.</p>
<p>Sidenote: It seems that matplotlib's <em>notebook</em> mode (which is for use in Jupyter Notebooks) doesn't work well with the live plotting, so we use <em>inline</em> mode.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[32]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">early_stopping</span> <span class="o">=</span> <span class="n">tf</span><span class="o">.</span><span class="n">keras</span><span class="o">.</span><span class="n">callbacks</span><span class="o">.</span><span class="n">EarlyStopping</span><span class="p">(</span><span class="n">monitor</span><span class="o">=</span><span class="s2">&quot;val_accuracy&quot;</span><span class="p">,</span> <span class="n">patience</span><span class="o">=</span><span class="mi">5</span><span class="p">,</span> <span class="n">restore_best_weights</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">close</span><span class="p">()</span>

<span class="n">history</span> <span class="o">=</span> <span class="n">model</span><span class="o">.</span><span class="n">fit</span><span class="p">(</span><span class="n">train_data</span><span class="p">,</span> 
                    <span class="n">train_classes</span><span class="p">,</span> 
                    <span class="n">batch_size</span><span class="o">=</span><span class="mi">32</span><span class="p">,</span> 
                    <span class="n">epochs</span><span class="o">=</span><span class="mi">100</span><span class="p">,</span> 
                    <span class="n">validation_data</span><span class="o">=</span><span class="p">(</span><span class="n">validation_data</span><span class="p">,</span> <span class="n">validation_classes</span><span class="p">),</span>
                    <span class="n">callbacks</span><span class="o">=</span><span class="p">[</span><span class="n">PlotLossesKeras</span><span class="p">(),</span> <span class="n">early_stopping</span><span class="p">])</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt"></div>




<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
"
>
</div>

</div>

<div class="output_area">

    <div class="prompt"></div>


<div class="output_subarea output_stream output_stdout output_text">
<pre>accuracy
	training         	 (min:    0.568, max:    0.984, cur:    0.980)
	validation       	 (min:    0.781, max:    0.897, cur:    0.890)
Loss
	training         	 (min:    0.055, max:    1.400, cur:    0.070)
	validation       	 (min:    0.409, max:    0.722, cur:    0.554)
875/875 [==============================] - 10s 12ms/step - loss: 0.0702 - accuracy: 0.9805 - val_loss: 0.5541 - val_accuracy: 0.8904
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>As we can see, during the training, the losses decrease and the accuracy increases.
After training, we can save our model for later use if we want to.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[6]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># save model</span>
<span class="n">model</span><span class="o">.</span><span class="n">save</span><span class="p">(</span><span class="n">datetime</span><span class="o">.</span><span class="n">now</span><span class="p">()</span><span class="o">.</span><span class="n">strftime</span><span class="p">(</span><span class="s2">&quot;</span><span class="si">%d</span><span class="s2">_%m_%Y__%H_%M&quot;</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;.h5&quot;</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[&nbsp;]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># load model</span>
<span class="n">model</span> <span class="o">=</span> <span class="n">keras</span><span class="o">.</span><span class="n">models</span><span class="o">.</span><span class="n">load_model</span><span class="p">(</span><span class="s2">&quot;05_08_2020__19_23.h5&quot;</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Another useful tool for evaluating a classifier's performance is a so called confusion matrix.
To compute the confusion matrix, we use our network to predict the class labels of all samples in the validation set.
The confusion matrix plots the probability with which a sample of a certain class is classified as belonging to a certain class. Thus, the values on the matrix' diagonal represent the correct classifications and those outside the diagonal the incorrect classifications. The matrix is thus by nanture symmetric.</p>
<p>The interesting thing to see is that the confusion matrix allows us to see if a certain pair of class labels are often falsely classified, i.e. confused with each other. If two classes would often be confused (e.g. because two words sound very similar) we would find a high value outside the diagonal. For example, if we look closely at the matrix below, we can see a slightly larger value (darker color) at "go"-"no". This means that these two words are more often confused with eachother, which is plausible since they sound very similar. The ideal result would be a value of $\frac1N$ ($N$=number of classes) on the diagonals (assuming classes are equally represented in the dataset) and zeros everywhere outside the diagonal.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[9]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># plot confusion matrix</span>
<span class="n">y</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">argmax</span><span class="p">(</span><span class="n">model</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">validation_data</span><span class="p">),</span> <span class="n">axis</span><span class="o">=</span><span class="mi">1</span><span class="p">)</span>
<span class="n">cm</span> <span class="o">=</span> <span class="n">confusion_matrix</span><span class="p">(</span><span class="n">validation_classes</span><span class="p">,</span> <span class="n">y</span><span class="p">,</span>  <span class="n">normalize</span><span class="o">=</span><span class="s2">&quot;all&quot;</span><span class="p">)</span>
<span class="o">%</span><span class="k">matplotlib</span> inline
<span class="n">plt</span><span class="o">.</span><span class="n">close</span><span class="p">()</span>
<span class="n">plt</span><span class="o">.</span><span class="n">figure</span><span class="p">(</span><span class="n">figsize</span> <span class="o">=</span> <span class="p">(</span><span class="mi">8</span><span class="p">,</span><span class="mi">8</span><span class="p">))</span>
<span class="n">plt</span><span class="o">.</span><span class="n">imshow</span><span class="p">(</span><span class="n">cm</span><span class="p">,</span> <span class="n">cmap</span><span class="o">=</span><span class="n">plt</span><span class="o">.</span><span class="n">cm</span><span class="o">.</span><span class="n">Blues</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">xlabel</span><span class="p">(</span><span class="s2">&quot;Predicted labels&quot;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">ylabel</span><span class="p">(</span><span class="s2">&quot;True labels&quot;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">xticks</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">arange</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span> <span class="mi">20</span><span class="p">,</span> <span class="mi">1</span><span class="p">),</span> <span class="n">index2word</span><span class="p">,</span> <span class="n">rotation</span><span class="o">=</span><span class="mi">90</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">yticks</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">arange</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span> <span class="mi">20</span><span class="p">,</span> <span class="mi">1</span><span class="p">),</span> <span class="n">index2word</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">tick_params</span><span class="p">(</span><span class="n">labelsize</span><span class="o">=</span><span class="mi">12</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">title</span><span class="p">(</span><span class="s1">&#39;Confusion matrix &#39;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">colorbar</span><span class="p">()</span>
<span class="n">plt</span><span class="o">.</span><span class="n">show</span><span class="p">()</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt"></div>




<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Ok, now we can try the keyword recognizer ourselves!</p>
<p>To easily record and play audio, we'll use the library <a href="https://python-sounddevice.readthedocs.io/en/0.4.0/index.html">sounddevice</a>. One thing to consider is, that we have created our CNN model so that it accepts an input feature vector that corresponds to an audio snippet of exactly 1s length at 16kHz sampling rate, i.e. 16000 samples. So we could record for exactly 1s, but this is not very practical, as you would have to say the word just at the right time after starting the recording so that it lies within the 1s time window.<br>
A more elegant solution is to record for a longer duration, e.g. 3s and then extract a 1s long snippet which we can then feed to our CNN. For this simple case we'll assume that the user says only one word during the recording, so we extract the 1s long snippet of the recording which contains the maximum signal energy. This sounds complicated, but can be quite easily computed using a <a href="https://en.wikipedia.org/wiki/Convolution">convolution</a>. First, we compute the power signal by element-wise squaring the audio signal. Then we create a 1s (i.e. 16000 points) long rectangle window and convolve the power signal with the window. We use <a href="https://numpy.org/doc/stable/reference/generated/numpy.convolve.html">"valid" mode</a> which means that only points where the signals overlap completely are computed (i.e. no zero-padding). This way, by computing the time at which the convolution is maximal, we get the starting time of the rectangle window which leads to maximal signal energy in the extracted snippet. We can then extract a 1s long snippet from the recording.</p>
<p>After defining a function to extract the 1s snippet, we configure the samplerate and device for recording. You can find out the number of the devices via <em>sd.query_devices()</em>. After recording for 3s and extracting the 1s snippet we can play it back. Then we compute the MFCC features and add a "fake" batch dimension to our sample before feeding it into our CNN mmodel for prediction. This is needed because the model expects batches of $\geq1$ samples as input, so since we have only one sample, we append a dimension to get a batch of one single sample. Additionally, we'll time the computation and model prediction to see how fast it is. We can normalize the CNN model's output to get a probability distribution (not strictly mathematical, but we can interpret it that way). Then we get the 3 candidates with highest probability and print the result. We'll also plot the raw audio signal and visulize the MFC coefficients.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[10]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">extract_loudest_section</span><span class="p">(</span><span class="n">audio</span><span class="p">,</span> <span class="n">length</span><span class="p">):</span>
    <span class="n">audio</span> <span class="o">=</span> <span class="n">audio</span><span class="p">[:,</span> <span class="mi">0</span><span class="p">]</span><span class="o">.</span><span class="n">astype</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">float</span><span class="p">)</span> <span class="c1"># to avoid integer overflow when squaring</span>
    <span class="n">audio_pw</span> <span class="o">=</span> <span class="n">audio</span><span class="o">**</span><span class="mi">2</span> <span class="c1"># power</span>
    <span class="n">window</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">ones</span><span class="p">((</span><span class="n">length</span><span class="p">,</span> <span class="p">))</span>
    <span class="n">conv</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">convolve</span><span class="p">(</span><span class="n">audio_pw</span><span class="p">,</span> <span class="n">window</span><span class="p">,</span> <span class="n">mode</span><span class="o">=</span><span class="s2">&quot;valid&quot;</span><span class="p">)</span>
    <span class="n">begin_index</span> <span class="o">=</span> <span class="n">conv</span><span class="o">.</span><span class="n">argmax</span><span class="p">()</span>
    <span class="k">return</span> <span class="n">audio</span><span class="p">[</span><span class="n">begin_index</span><span class="p">:</span><span class="n">begin_index</span><span class="o">+</span><span class="n">length</span><span class="p">]</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[11]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">sd</span><span class="o">.</span><span class="n">default</span><span class="o">.</span><span class="n">samplerate</span> <span class="o">=</span> <span class="mi">16000</span>
<span class="n">sd</span><span class="o">.</span><span class="n">default</span><span class="o">.</span><span class="n">channels</span> <span class="o">=</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">2</span> <span class="c1"># mono record, stereo playback</span>


<span class="n">recording</span> <span class="o">=</span> <span class="n">sd</span><span class="o">.</span><span class="n">rec</span><span class="p">(</span><span class="nb">int</span><span class="p">(</span><span class="mi">3</span><span class="o">*</span><span class="n">sd</span><span class="o">.</span><span class="n">default</span><span class="o">.</span><span class="n">samplerate</span><span class="p">),</span> <span class="n">channels</span><span class="o">=</span><span class="mi">1</span><span class="p">,</span> <span class="n">samplerate</span><span class="o">=</span><span class="n">sd</span><span class="o">.</span><span class="n">default</span><span class="o">.</span><span class="n">samplerate</span><span class="p">,</span> <span class="n">dtype</span><span class="o">=</span><span class="n">np</span><span class="o">.</span><span class="n">float</span><span class="p">,</span> <span class="n">blocking</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>
<span class="n">recording</span> <span class="o">=</span> <span class="n">extract_loudest_section</span><span class="p">(</span><span class="n">recording</span><span class="p">,</span> <span class="nb">int</span><span class="p">(</span><span class="mi">1</span><span class="o">*</span><span class="n">sd</span><span class="o">.</span><span class="n">default</span><span class="o">.</span><span class="n">samplerate</span><span class="p">))</span> <span class="c1"># extract 1s snippet with highest energy (only necessary if recording is &gt;3s long)</span>
<span class="n">sd</span><span class="o">.</span><span class="n">play</span><span class="p">(</span><span class="n">recording</span><span class="p">,</span> <span class="n">blocking</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>

<span class="n">t1</span> <span class="o">=</span> <span class="n">timer</span><span class="p">()</span>
<span class="n">recorded_feature</span> <span class="o">=</span> <span class="n">audio2feature</span><span class="p">(</span><span class="n">recording</span><span class="p">)</span>
<span class="n">t2</span> <span class="o">=</span> <span class="n">timer</span><span class="p">()</span>
<span class="n">recorded_feature</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">expand_dims</span><span class="p">(</span><span class="n">recorded_feature</span><span class="p">,</span> <span class="mi">0</span><span class="p">)</span> <span class="c1"># add &quot;fake&quot; batch dimension 1</span>
<span class="n">prediction</span> <span class="o">=</span> <span class="n">model</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">recorded_feature</span><span class="p">)</span><span class="o">.</span><span class="n">reshape</span><span class="p">((</span><span class="mi">20</span><span class="p">,</span> <span class="p">))</span>
<span class="n">t3</span> <span class="o">=</span> <span class="n">timer</span><span class="p">()</span>
<span class="c1"># normalize prediction output to get &quot;probabilities&quot;</span>
<span class="n">prediction</span> <span class="o">/=</span> <span class="n">prediction</span><span class="o">.</span><span class="n">sum</span><span class="p">()</span>

<span class="c1"># print the 3 candidates with highest probability</span>
<span class="n">prediction_sorted_indices</span> <span class="o">=</span> <span class="n">prediction</span><span class="o">.</span><span class="n">argsort</span><span class="p">()</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;candidates:</span><span class="se">\n</span><span class="s2">-----------------------------&quot;</span><span class="p">)</span>
<span class="k">for</span> <span class="n">k</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="mi">3</span><span class="p">):</span>
    <span class="n">i</span> <span class="o">=</span> <span class="nb">int</span><span class="p">(</span><span class="n">prediction_sorted_indices</span><span class="p">[</span><span class="o">-</span><span class="mi">1</span><span class="o">-</span><span class="n">k</span><span class="p">])</span>
    <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;</span><span class="si">%d</span><span class="s2">.)</span><span class="se">\t</span><span class="si">%s</span><span class="se">\t</span><span class="s2">:</span><span class="se">\t</span><span class="si">%2.1f%%</span><span class="s2">&quot;</span> <span class="o">%</span> <span class="p">(</span><span class="n">k</span><span class="o">+</span><span class="mi">1</span><span class="p">,</span> <span class="n">index2word</span><span class="p">[</span><span class="n">i</span><span class="p">],</span> <span class="n">prediction</span><span class="p">[</span><span class="n">i</span><span class="p">]</span><span class="o">*</span><span class="mi">100</span><span class="p">))</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;-----------------------------&quot;</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;feature computation time: </span><span class="si">%2.1f</span><span class="s2"> ms&quot;</span> <span class="o">%</span> <span class="p">((</span><span class="n">t2</span><span class="o">-</span><span class="n">t1</span><span class="p">)</span><span class="o">*</span><span class="mf">1e3</span><span class="p">))</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;CNN model prediction time: </span><span class="si">%2.1f</span><span class="s2"> ms&quot;</span> <span class="o">%</span> <span class="p">((</span><span class="n">t3</span><span class="o">-</span><span class="n">t2</span><span class="p">)</span><span class="o">*</span><span class="mf">1e3</span><span class="p">))</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;total time: </span><span class="si">%2.1f</span><span class="s2"> ms&quot;</span> <span class="o">%</span> <span class="p">((</span><span class="n">t3</span><span class="o">-</span><span class="n">t1</span><span class="p">)</span><span class="o">*</span><span class="mf">1e3</span><span class="p">))</span>

<span class="n">plt</span><span class="o">.</span><span class="n">close</span><span class="p">()</span>
<span class="n">plt</span><span class="o">.</span><span class="n">figure</span><span class="p">(</span><span class="mi">1</span><span class="p">,</span> <span class="n">figsize</span><span class="o">=</span><span class="p">(</span><span class="mi">10</span><span class="p">,</span> <span class="mi">7</span><span class="p">))</span>
<span class="n">plt</span><span class="o">.</span><span class="n">subplot</span><span class="p">(</span><span class="mi">211</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">recording</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">subplot</span><span class="p">(</span><span class="mi">212</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">imshow</span><span class="p">(</span><span class="n">recorded_feature</span><span class="o">.</span><span class="n">reshape</span><span class="p">(</span><span class="mi">99</span><span class="p">,</span> <span class="mi">20</span><span class="p">)</span><span class="o">.</span><span class="n">T</span><span class="p">,</span> <span class="n">aspect</span><span class="o">=</span><span class="s2">&quot;auto&quot;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">show</span><span class="p">()</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt"></div>


<div class="output_subarea output_stream output_stdout output_text">
<pre>candidates:
-----------------------------
1.)	yes	:	99.8%
2.)	no	:	0.1%
3.)	left	:	0.0%
-----------------------------
feature computation time: 5.3 ms
CNN model prediction time: 92.4 ms
total time: 97.7 ms
</pre>
</div>
</div>

<div class="output_area">

    <div class="prompt"></div>




<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>As we see in this case, the results look really good. If the probability for the best candidate is very high and those of the second-best and third-best candidates are pretty low, the prediction seems quite trustworthy.<br>
Additionally, we can see that the feauture computation and CNN model prediction are quite fast. The total execution time is around 100ms, which means that our method is quite able to work in "real-time".</p>
<p>So now let's adapt and extend this little demo to work in real-time. For this, we'll use a buffer that contains 5 succeeding snippets of 3200 samples, i.e. 200ms each. We implement this audio buffer as a ringbuffer, which means that every time a new 200ms long snippet has been recorded, the oldest snippet in the buffer is discarded, the buffer is moved one step back and the newest snippet is put at the last position. This way, our buffer is updated every 200ms and always contains the last 1s of recorded audio. Since our prediction takes approximately 100ms and we have 200ms between each update, we have enough time for computation and achieve a latency of &lt;200ms (so I think it can be considered "real time" in this context).</p>
<p>To implement the buffer in python, we can make use of numpy's <a href="https://numpy.org/doc/stable/reference/generated/numpy.roll.html">roll()</a> function. We roll our buffer with a step of -1 along the first axis, which means that all 5 snippets are shifted to the left and the first snippet rolls over to the last position. Then we replace the snippet at the last position (which is the oldest snippet we whish to discard) with the newest snippet.</p>
<p>We define a callback function with an appropriate signature for the sounddevice Stream API (see <a href="https://python-sounddevice.readthedocs.io/en/0.4.0/api/streams.html#sounddevice.Stream">here</a>) that updates the audio buffer and makes a new prediction each time a new snippet is recorded. We use a simple threshold of 70% probability to check if a word has been recognized. When a word is recognized, it will also appear in the buffer after the next couple of updates, so it will be recognized more than once in a row. To avoid this, we can implement a timeout that ignores a recognized word, if the same word has already been recognized shortly before.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[14]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">audio_buffer</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">zeros</span><span class="p">((</span><span class="mi">5</span><span class="p">,</span> <span class="mi">3200</span><span class="p">))</span>
<span class="n">last_recognized_word</span> <span class="o">=</span> <span class="kc">None</span>
<span class="n">last_recognition_time</span> <span class="o">=</span> <span class="mi">0</span>
<span class="n">recognition_timeout</span> <span class="o">=</span> <span class="mf">1.0</span>

<span class="k">def</span> <span class="nf">audio_stream_callback</span><span class="p">(</span><span class="n">indata</span><span class="p">,</span> <span class="n">frames</span><span class="p">,</span> <span class="n">time</span><span class="p">,</span> <span class="n">status</span><span class="p">):</span>
    <span class="k">global</span> <span class="n">audio_buffer</span>
    <span class="k">global</span> <span class="n">model</span>
    <span class="k">global</span> <span class="n">index2word</span>
    <span class="k">global</span> <span class="n">last_recognized_word</span>
    <span class="k">global</span> <span class="n">last_recognition_time</span>
    <span class="n">audio_buffer</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">roll</span><span class="p">(</span><span class="n">audio_buffer</span><span class="p">,</span> <span class="n">shift</span><span class="o">=-</span><span class="mi">1</span><span class="p">,</span> <span class="n">axis</span><span class="o">=</span><span class="mi">0</span><span class="p">)</span>
    <span class="n">audio_buffer</span><span class="p">[</span><span class="o">-</span><span class="mi">1</span><span class="p">,</span> <span class="p">:]</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">squeeze</span><span class="p">(</span><span class="n">indata</span><span class="p">)</span>
    <span class="n">t1</span> <span class="o">=</span> <span class="n">timer</span><span class="p">()</span>
    <span class="n">recorded_feature</span> <span class="o">=</span> <span class="n">audio2feature</span><span class="p">(</span><span class="n">audio_buffer</span><span class="o">.</span><span class="n">flatten</span><span class="p">())</span>
    <span class="n">recorded_feature</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">expand_dims</span><span class="p">(</span><span class="n">recorded_feature</span><span class="p">,</span> <span class="mi">0</span><span class="p">)</span> <span class="c1"># add &quot;fake&quot; batch dimension 1</span>
    <span class="n">t2</span> <span class="o">=</span> <span class="n">timer</span><span class="p">()</span>
    <span class="n">prediction</span> <span class="o">=</span> <span class="n">model</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">recorded_feature</span><span class="p">)</span><span class="o">.</span><span class="n">reshape</span><span class="p">((</span><span class="mi">20</span><span class="p">,</span> <span class="p">))</span>
    <span class="c1"># normalize prediction output to get &quot;probabilities&quot;</span>
    <span class="n">prediction</span> <span class="o">/=</span> <span class="n">prediction</span><span class="o">.</span><span class="n">sum</span><span class="p">()</span>
    <span class="c1">#print(prediction)</span>
    <span class="n">best_candidate_index</span> <span class="o">=</span> <span class="n">prediction</span><span class="o">.</span><span class="n">argmax</span><span class="p">()</span>
    <span class="n">best_candidate_probability</span> <span class="o">=</span> <span class="n">prediction</span><span class="p">[</span><span class="n">best_candidate_index</span><span class="p">]</span>
    <span class="n">t3</span> <span class="o">=</span> <span class="n">timer</span><span class="p">()</span>
    <span class="k">if</span><span class="p">(</span><span class="n">best_candidate_probability</span> <span class="o">&gt;</span> <span class="mf">0.7</span><span class="p">):</span> <span class="c1"># treshold</span>
        <span class="n">word</span> <span class="o">=</span> <span class="n">index2word</span><span class="p">[</span><span class="n">best_candidate_index</span><span class="p">]</span>
        <span class="k">if</span><span class="p">(</span> <span class="p">(</span><span class="n">timer</span><span class="p">()</span><span class="o">-</span><span class="n">last_recognition_time</span><span class="p">)</span><span class="o">&gt;</span><span class="n">recognition_timeout</span> <span class="ow">or</span> <span class="n">word</span><span class="o">!=</span><span class="n">last_recognized_word</span> <span class="p">):</span>
            <span class="n">last_recognition_time</span> <span class="o">=</span> <span class="n">timer</span><span class="p">()</span>
            <span class="n">last_recognized_word</span> <span class="o">=</span> <span class="n">word</span>
            <span class="n">clear_output</span><span class="p">(</span><span class="n">wait</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span> <span class="c1"># clear ouput as soon as new output is available to replace it</span>
            <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;</span><span class="si">%s</span><span class="se">\t</span><span class="s2">:</span><span class="se">\t</span><span class="si">%2.1f%%</span><span class="s2">&quot;</span> <span class="o">%</span> <span class="p">(</span><span class="n">word</span><span class="p">,</span> <span class="n">best_candidate_probability</span><span class="o">*</span><span class="mi">100</span><span class="p">))</span>
            <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;-----------------------------&quot;</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Now we can finally start the real-time demo of our CNN keyword recognizer. Therefore we start an input stream which calls our callback function each time a new block of 3200 samples has been recorded. We'll let the recognizer run for one minute so we have plenty of time to try it out.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[16]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># REALTIME KEYWORD RECOGNITION DEMO (60s long)</span>
<span class="k">with</span> <span class="n">sd</span><span class="o">.</span><span class="n">InputStream</span><span class="p">(</span><span class="n">samplerate</span><span class="o">=</span><span class="mi">16000</span><span class="p">,</span> <span class="n">blocksize</span><span class="o">=</span><span class="mi">3200</span><span class="p">,</span> <span class="n">device</span><span class="o">=</span><span class="kc">None</span><span class="p">,</span> <span class="n">channels</span><span class="o">=</span><span class="mi">1</span><span class="p">,</span> <span class="n">dtype</span><span class="o">=</span><span class="s2">&quot;float32&quot;</span><span class="p">,</span> <span class="n">callback</span><span class="o">=</span><span class="n">audio_stream_callback</span><span class="p">):</span>
    <span class="n">sd</span><span class="o">.</span><span class="n">sleep</span><span class="p">(</span><span class="mi">60</span><span class="o">*</span><span class="mi">1000</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt"></div>


<div class="output_subarea output_stream output_stdout output_text">
<pre>on	:	99.2%
-----------------------------
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[&nbsp;]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span> 
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[&nbsp;]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span> 
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[&nbsp;]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span> 
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[&nbsp;]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span> 
</pre></div>

    </div>
</div>
</div>

</div>
    </div>
  </div>
</body>

 


</html>
