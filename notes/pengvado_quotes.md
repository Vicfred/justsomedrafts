(Shamelessly copied from the similar Michael Niedermayer page)
How can one explain the phenomenon that is Loren Merritt? Perhaps he is a superintelligent alien sent to observe us, who hacks on x264 and ffmpeg for amusement. It's also possible he's either a cybernetically enhanced human, or a wildly successful AI project. Either way, there must be great wisdom contained in his writing. Perhaps most of this is contained in code. But for laypeople, studying some of his quotes might provide some benefits.

I have tried to identify the most prototypical Loren quotes. A large fraction of his messages fall into one of the formats below. A few of the posts highlighted below are special and don't belong into any large category.

Here are a few top ranking candidates. IT IS VERY IMPERATIVE THAT YOU READ EACH AND EVERY LETTER OF EACH AND EVERY ONE.

Loren Merritt...
On Algorithms
On Broadcast and Media
On Coding
On Copyright
On Competitors
On Development
On GCC
On Hardware
On Linux
On Quality
On Touhou
On Windows
Miscellaneous



On Algorithms
<Dark_Shikari> ok, its a motion estimation algorithm in a paper
<Dark_Shikari> 99% chance its totally useless
<wally4u> because?
<pengvado> because there have been about 4 useful motion estimation papers ever, and a lot more than 400 attempts

<pengvado> there's nothing wrong with a high level analysis of which areas are important. it's just that our strong AI isn't quite advanced enough to make it work :)
<Dark_Shikari> the thing is you don't only need an AI, you need a human-like AI, since you need to measure which areas a *human* would judge as important
<pengvado> oh right. we don't need an AI, we can just use the chained grad student strategy

<Dark_Shikari> DUSK was atrocious
<Dark_Shikari> ROOTS was not as bad as DUSK but pretty close
<Dark_Shikari> and yes, I call "legend of the twilight bracelet" DUSK because it doesn't deserve more than 4 letters of title.
<pengvado> that's backwards. good huffman coding dictates that only things you discuss frequently deserve short titles. and surely crap is discussed less frequently than good anime?

<pengvado> Dark_Shikari: so, did you traverse tvtropes depth-first or breadth-first? perhaps with alpha-beta pruning?

<Compn> The algorithm was implemented using open source H.264/AVC reference software JM10.1 and the simulation results show that encoding time of first 100 frames of various video sequences can be reduced up to 40 to 60 per cent using the new algorithm without significant increase in bit-rate and reduction in video quality.
<mru> reducing time by *only* 60% from JM is impressive in its own way
<Compn> some neat graphs in the pdf
<pengvado> I can draw neat graphs too. but I'm not sure winamp visualizations would be appropriate for an academic paper.

<mp4_guy> everything is linear if you look hard enough
<pengvado> unless you mean "the shroedinger equation is linear, so everything else in the universe is too", you're not looking too hard if you find linearity

<pengvado> of course. especially for fade-in, where coding one P and lots of b lets you code the residual just once, whereas multiple P even with wpred spreads out the refinement over time
<pengvado> the problem is that non-avisynth-generated fades aren't necessarily linear
<pengvado> an AI-complete codec would just make the fade linear and then code it optimally :)

(a year or so later)
<pengvado> why aren't fadeouts perfect?
<Dark_Shikari> because weightp isn't perfect, and not all fades are linear
<pengvado> oh right, well just get cracking on that AI problem
On Broadcast and Media
<CruNcher> i just can tell you that some of my idears are professionaly done and used in broadcast nowdays
<pengvado> Is "used in broadcast" supposed to be an endorsement?

<pengvado> do I have any reason to believe that you have the right/ability/marketshare to "design the future of internet television", as opposed to all the other people trying to do the same thing in different ways?

<User56> and dvd is 460i right?
<pengvado> 480i or 480p, whichever is appropriate to the content. unless the masterer is incompetent, in which case whichever is inappropriate to the content.
On Coding
<hito> no void * could point to anywhere. so if programmer could write it wrong code.
<pengvado> as opposed to int* which can't point anywhere?

<Dark_Shikari> __pleasefixmybadcode
<saintdev> Dark_Shikari: this _is_ microsoft, why on earth would they have that??
<pengvado> __pleaseborkmygoodcode ?

<pengvado> ok, the asm you pasted is fucking weird
<pengvado> and I'd bet it's compiler-generated, because no human could be that stupid
<pengvado> it's an 8x7 vsad
<pengvado> no, I don't know what use a decoder has for that

<pengvado> title[] is no longer a title, rename
<Dark_Shikari> to what, buf?
<pengvado> sure
<pengvado> generic and uninformative is better than wrong

<pengvado> I can't shake the feeling that there's something horribly wrong with applying a regex substitution to an exe
<pengvado> I reallocated struct elements by changing every instruction that contained a memory arg of [reg+x] for certain values of x
<Dark_Shikari> how did you know when that reg contained the address of the struct?
<pengvado> I didn't, I just hope that there aren't any instances of those x's that aren't the struct in question

<Dark_Shikari> wait, so you're saying my shifts are wrong?
<pengvado> dunno
<pengvado> if I'm right, then I don't know how you managed to get sane results with quant off by 8
<Dark_Shikari> so what are you saying I should do?
<pengvado> throw stuff at the wall and see what sticks

<pengvado> anyone who ever looked at cuda has run screaming into the night. it's like a tome of eldritch lore.

<riccardo_> STM is actually trying to do that
<riccardo_> i have a meeting with them next week
<riccardo_> but they sad they will probably not be able to make production ready code in the near future (this year)
<riccardo_> so it would be nice to have the x264 dev. to help us getting a mature code
<pengvado> did they factor in the part where I throw away all their code and reimplement it better? because "working on it for a year and won't be ready for another year" doesn't really inspire much confidence in their codebase

<checkers> "follow this 1000 step procedure to get the number of physical cores"
<pengvado> follow this n step(*) procedure to get the number of physicalcores. * n = number of cores, must be known in advance.

<pengvado> mvd_diff, from the department of redundancy department.

<Dark_Shikari> how about psubq?
<pengvado> wow, I totally forgot that 64bit int simd existed

<pengvado> nv12 c finished. now to update the asm...
<kemuri-_9> when do you expect to get that done pengvado?
<pengvado> how am I supposed to be a deadly multimedia ninja if people go and *expect* me?

<pengvado> sad-a.asm has 3 lines of Laurent code, which are %macro, %endmacro, and a blank
On Copyright, Patents, and Copy Protection
<pengvado> which genius came up with the idea of isolation copy protection in its own exe, thus making it trivial to bypass?
<sysKin> where's such a thing? :)
<pengvado> it's called alpharom, used in many japanese games
<pengvado> so, some dev said "hey, we can make the easiest protection ever. it'll be all sophisticated and stuff, with cd checks, anti-debugger checks, and everything. then we'll practically add a button that says "click here to disable copy protection""
<pengvado> ok, so prism ark has all the files associated with alpharom but forgot to actually enable it

<Dark_Shikari> copyright law doesn't affect reverse-engineering
<kshishkov|work> it does
<pengvado> you mean I wasn't supposed to paste all that asm from coreavc? :)
<Dark_Shikari> wait, did you actually take any directly from coreavc?
<pengvado> no, I read it, then found a better way to write it
<pengvado> mc, hct, intra

<Dark_Shikari> .... yet was granted anyways, despite blatant evidence of prior art
<pengvado> of course. anything cited by a patent doesn't count as prior art. </cynic>
<pengvado> for that matter, anything that isn't a patent doesn't count as prior art

<pengvado> the question of LGPL mainly resolves around how pissed off people will be when I delete working LGPL code to add faster GPL code

<CIA-25> ffmpeg: lorenm * r16845 /trunk/libavcodec/vc1dsp.c: fix an overflow in vc1 intra overlap filter
<Dark_Shikari> zomg loren made a commit to vc1
*kshishkov|work looks at Dark_Shikari
<pengvado> a starcraft video had artifacts. they were annoying. so I fixed them.
<pengvado> and no, I haven't read the spec. I just made it do the right thing.

* mru is tempted to #define TRUE 2 for *some* files
<Dark_Shikari> mru: #define FALSE 1
<Dark_Shikari> #define TRUE 0
<pengvado> #define TRUE (!FALSE)
<pengvado> #define FALSE (!TRUE)
On Competitors
<Dark_Shikari> apparently DivX beta decoder is a LOT better than CoreAVC the more threads you add (at least according to their graphs) (for 4 or 8 cores)
<Dark_Shikari> for single CPU difference is marginal
<Dark_Shikari> oddly, they disabled AQ when encoding their streams
<pengvado> they have a deblocking optimization that depends on constant qp. </cynic>

<|bond|> i wonder why the good encoders never make it into ffmpeg directly
<pengvado> plural? which was the other good encoder?

<Dark_Shikari> "our encoder works 2x realtime on 720p. That's 8 times faster than a 3Ghz quadcore!"
<Dark_Shikari> I'm not sure what encoder they're claiming they're 8 times faster than... maybe the JM?
<Dark_Shikari> (x264 is 4x realtime on 720p on their 3Ghz quadcore, on lowest settings)
<pengvado> you say that as if there's any commercial product that isn't 8x faster than the competition :)

<pengvado> iirc xvid's "vhq" option was copied from ffmpeg's "vhq", without knowing that the "v" was simply ffmpeg's prefix for all options that affected video, and the real name was "hq"

<pengvado> <xxthink> but why, such as mainconcept, can't produce a much better product than x264?
<pengvado> because they're a company, and we're a couple of guys in our basements. obviously, in such a competition, the couple of guys win.
On Development
<pengvado> making an alpha product into final is easy
<pengvado> the hard part is adding features so that it stays alpha

<pengvado> "any decent size project contains a lisp interpreter"

<Dark_Shikari> akupenguin == pengvado
<Shinigami-Sama> I was not sure of that
<Shinigami-Sama> I remember someone said he wasn't a long time ago
<Dark_Shikari> yes, he is
<Dark_Shikari> I mean seriously, being one of only two particularly active x264 developers, the other being him
<Dark_Shikari> I better know =p
<Dark_Shikari> if I didn't there would be a serious problem
<pengvado> don't be silly. there are 4 active x264 developers. loren, pengvado, aku, and ds

(from ~2005)
<OpenSourced> where do you think x264 will be in 3 years
<pengvado> replaced by snow :)

<Dark_Shikari> commit now or wait?
<pengvado_> encode something and see that it works
<Dark_Shikari> k
<pengvado> but other than that, commit it now before deaththesheep decides the bugger version was better

<Dark_Shikari> also, x264 is getting adaptive MBAFF support Soon(TM)
<Dark_Shikari> then we'll be able to encode 98% of interlaced material as progressive anyways
<pengvado> does the mbaff get to decide "this block is better off deinterlaced"? :)

<Dark_Shikari> is the code otherwise correct?
<pengvado> you say that as if I have magical bug-seeking skillz
<saintdev|laptop> <pengvado> you say that as if I have magical bug-seeking skillz <--you don't??
<pengvado> only on home ground. I don't have magical skillz for seeking bugs in other people's code.

<Dark_Shikari> --csp 444?
<pengvado> ooh, can we support 4:2:1 and 4:4:3 just to confuse everyone?
<Dark_Shikari> 4:2:1?! wtf is that
<pengvado> chroma gets a triangular subsampling grid, not the usual square lattice
<pengvado> you know, triangular grid is better in terms of accurately representing every point in space by a limited number of samples. it's only these pesky raster displays and separable dcts that get in the way :)

<Dark_Shikari> why isn't it committed? is it bugged, or what?
<pengvado> yes
<Dark_Shikari> I thought you had magical bug-seeking skillz
<pengvado> of course I do, but laziness is a cardinal virtue

<pengvado> are there any devices that support 25 mbit high profile (L4.0), but don't support 25mbit main profile (L4.1) ?
<Dark_Shikari> no idea, but its easier to blanket-apply the rule to everything than to special-case
<Dark_Shikari> you can never go wrong by abiding by the spec, since if someone breaks the spec you can just blame them instead
<pengvado> fair enough. I'll allow the patch on the premise of passing the blame.

<Dark_Shikari> every time someone comes in and says you should use (haskell|ruby|C#|java|C++|brainfuck) for x264
<Dark_Shikari> we must require they to give one example of an enormously useful feature from that language that would be extremely helpful to developers of x264.
<pengvado> too specific. how about just autoban for mentioning java?

<Dark_Shikari> k, time to write the enormous commit message
<pengvado> really? normally commit message length is inversely proportional to commit length, unless it's just an amalgamation of lots of independent patches. I mean, if I were to commit all of x264 at once, it would just say "new h264 encoder". :)

<pengvado> xiph aren't developers, so discussion thereof doesn't go in the development channel

<Dark_Shikari> I think we've found more bugs in each others' code and our own in the past week than ever before
<Dark_Shikari> (my theory is that MB-tree has magical bug-hiding properties)
<pengvado> well, it *is* so awesome that even a buggy version is still great...

<Dark_Shikari> pengvado: <link to patch omitted>
<pengvado> generally ok, ask again tomorrow

<kokotier> so you have have an idea when H265 spec will be ready ?
<pengvado> before it's ready

<Dark_Shikari> irock: you have three sets of comments to deal with
<Dark_Shikari> 1) the ones before the 4000-series I dealt with (my code)
<Dark_Shikari> 2) the ones at the end of the 4000-series (after my code)
<Dark_Shikari> 3) pengvado's latest spaaaaam
<pengvado> 4) the ones I haven't written yet

<Dark_Shikari> 24 commits now (they're breeding, like gremlins in water)
<pengvado> what happens if I feed them?

<Dark_Shikari> pengvado: any objections to http://pastebin.com/40JC1aTv?
<Dark_Shikari> yes its silly but a customer requested it.
<pengvado> that... actually makes sense. patch ok.
On GCC
<Dark_Shikari> are there intrinsics one can give gcc to cause it not to do retarded things?
<pengvado> __attribute__((no_retard))

<pengvado> I got -fwhole-program working, but it doesn't give any measurable speedup
<saintdev> do you have -fomg-fast-speed working yet?
<pengvado> -fomit-instructions?

<pengvado> gcc fails to optimize it because gcc has always sucked at arrays
<pengvado> that's *the* benefit of fortran

<pengvado> will eventually need a struct mv_t to simplify int16_t[2] manipulations
<pengvado> hmm, no it fails
<pengvado> simple assignment doesn't work either, that also compiles to assignment of member fields
<pengvado> ok, gcc sucks at structs just as much as it sucks at arrays. scratch that idea.

<pengvado> all the standard tools suck. I need to fix copy+paste in uuterm so I can switch from xterm, and I need to fix wildcards in psh so I can switch from bash, and I need to write a C compiler so I can switch from gcc ...

<Dark_Shikari> so why am I losing so much speed?
<Dark_Shikari> wouldn't it compile to the exact same thing?!
<Dark_Shikari> given that its inlined
<pengvado> gcc sucks?

<pengvado> ICE not crash. that is, it reports an error in what really shouldn't be an error condition, then refuses to compile your code.
<pengvado> this case is slightly believable, as -freorder-blocks-and-partition and -fprofile-use both modify sections (hot vs cold functions), and neither is implied by any of the -O settings so it's quite possible that they were never tested together
<pengvado> oops, this doesn't depend on -Os. just those 2
<pengvado> oh well. typical oss courtesy says I should file a bugreport, but I'm really not interested in dealing with gcc people, so I'll just file it under "don't do that"

<Dark_Shikari> WHAT THE HELL I change the flat to zeroes and it segfaults on startup on my machine?!?!!
<Dark_Shikari> In fact I've found this everywhere now, anywhere I use a static array it crashes.
<pengvado> x264 must have been added to the gcc testsuite, so they have a pristine copy to compare against and know when to crash

<Dark_Shikari> did you hear they're adding the ability to inline function pointers in gcc?
<pengvado> of course. because "interesting" optimizations are preferred over optimizations that make programs fast

<Dark_Shikari> is there a measurable cost, in general to always-correctly-predicted branches in inner loops?
<pengvado> depends which side of the loop is inlined
<pengvado> iirc k8 has a minimum cost of 2 cycles for a jump of any kind but not for a non-taken branch
<Dark_Shikari> so in profiled mode, it'll inline the non-lossless. in non-profiled, how do you know which side it will?
<pengvado> flip a coin?

<pengvado> as you noticed, gcc doesn't store array elements in registers
<Dark_Shikari> even extremely simple arrays, like x[2]?
<pengvado> oh, gcc does, but only when it doesn't help
<Dark_Shikari> Is that like Murphy's Law of gcc--it only does useful things when they aren't useful?
<pengvado> e.g. struct mv { int16_t x[2]; } does store x[2] in registers, thus preventing write combining

<Dark_Shikari> is there any good reason why the makefile has -O4 in it?
<pengvado> because 4 is bigger than 3, duh

<Dark_Shikari> it's much slower on gcc 3.4
<Dark_Shikari> 100 -> 130 cycles
<pengvado> anything obvious in the asm?
<pengvado> because all this is really just rerolling the gcc random code generator

<Dark_Shikari> What happened there is exactly equivalent to the following:
<Dark_Shikari> last_nonb = i; i--; cur_nonb = i;
<Dark_Shikari> assert( last_nonb != cur_nonb );
<Dark_Shikari> That assert failed. This is, of course, completely impossible.
<pengvado> We're talking gcc here. It does the impossible every morning before breakfast.
On Hardware
<Dark_Shikari> <Snowknight26> but but but
<Dark_Shikari> <Snowknight26> thats what i do all my encoding on
<Dark_Shikari> (referring to a pentium 1 machine)
<pengvado> it's obviously no hardship for him. his (singular) encode won't finish for a while yet, so he can't have upgraded to the new x264 :)

<Dark_Shikari> it crashes with gcc 3.4 too
<Dark_Shikari> pengvado: what the hell should I do?
<pengvado> exorcise your computer

*pengvado runs x264, notices SSE4
<pengvado> wow, I got a penryn while I wasn't looking
<pengvado> guess those optimizations will happen sooner than I planned

<holger> btw, we don't care that much about p4, right? i have a couple movq because they're cheap on core. but they'll hurt p4.
<holger> (movq mmX, mmX)
<pengvado> the p4 never existed

<jarod> "please make a new x264 mod build (with PsyRD 0.3 if possible) ASAP. the new git version has a critical fix."
<jarod> what critical?
<Dark_Shikari> critical for people on shitty cpus like the athlon 64
<pengvado> no, for people running shitty osses that don't even allow you to make use of the full x86_64ness of the half-decent cpu which is athlon64

<Setsuna-Xero> so, I was working on a PC for a client
<Setsuna-Xero> I got like 6 shot glasses of dust out of it
<Setsuna-Xero> and found out the Harddrive is totally fried
<Setsuna-Xero> 3 working sectors, 2 of them were bad data in 15 min of a scan
<pengvado> why were you collecting dust into shot glasses?

<Dark_Shikari> nope, still slower than pure ssse3
<Dark_Shikari> so we'll have to wait on the phenom results.
<Dark_Shikari> you know, if amd was smart, they'd give us access to a phenom box :/
<pengvado> have you tried asking?

<CruNcher> spp works crazy good on old cinepak stuff :)
<checkers> it's still like using depleted uranium tank rounds against an eeepc
<pengvado> for when you want your notebook really dead?

<Yuvi> unpack/interleave: 2/3 (128 bit)
<Dark_Shikari> wait, throughput is worse than latency?
<Dark_Shikari> that cannot physically be possible
<pengvado> the shift unit gets tired and has to rest for 1 cycle

<pengvado> haruhi didn't like the p4, so she retroactively unreleased it

<holger> hmm. i finally read up on the avx documentation. unless i'm missing something, the only 256 bit integer instruction is PTEST. well done intel. i doubt the three operand format alone is going to gain much.
<holger> avx is pretty much float only
<Dark_Shikari> are they on crack?
<Dark_Shikari> pengvado: since you have those intel contacts, what say they?
<pengvado> err, no. it didn't occur to me to ask "will avx be completely useless"...
On Linux
<pengvado> argh, is there any way to increase the maximum length of a commandline?
<pengvado> linux won't let me put more than 128KB on one line

<pengvado> `valgrind valgrind x264`
<pengvado> valgrind: You cannot run 'valgrind' directly.
<pengvado> valgrind: You should use $prefix/bin/valgrind.

<pengvado> aw, I can't make a single malloc request for more memory than I have physical ram. I though linux did lazy allocation?

<Dark_Shikari> so this whole getopt thing is part of libc? it seems a rather fancy thing to end up in common libc.
<pengvado> lots of programs need to parse commandlines. yes it's in posix.
<Dark_Shikari> it just seems that it has a ton of global variables and stuff, the kind of thing that C programmers beat you over the head for doing.
<Dark_Shikari> and they're in the global namespace too.
<pengvado> and why do they know to beat you over the head? because they did it for years in all the classical unices.
On Quality
<pengvado> Of course it doesn't look better than the source. It's --deblock, not --pixie-dust.

<Dark_Shikari> ugh, you saw the guy in the big buck bunny thread?
<Dark_Shikari> he justified his bitrate of 15 megabits (WAY above transparency) by saying he wanted "the true hi-def experience"
<Dark_Shikari> the marketing seems to be working
<pengvado> you're just missing the definition of "the true hi-def experience". it really means "requiring a bigger hdd"

<ChronoCross> I feel as though ----------------------tesa should be an option. it should be compareable to lossless with the side effect of it taking an eternity to encode.
<ChronoCross> I guarantee 90% of the people in this channel and doom9 would use it.
<pengvado> no, it has to be blatantly obvious.
<pengvado> --placebo
<pengvado> which makes your encode 10x slower and donates the difference to folding@home

<`Orum> do you think esa is ever worth it over umh?
<Dark_Shikari> sure, if you reach a certain level of not caring about speed
<Dark_Shikari> though personally I prefer tesa, i.e. if esa is worth it, tesa probably is too.
<`Orum> I thought tesa was much slower than esa
<Dark_Shikari> no, 20% or so
<`Orum> ouch
<pengvado> if you ouch at 20%, esa is not for you
On Touhou
<pengvado> nice of touhou to put the borders of the active window on mod16 positions :)

<pengvado> wow, does swr use lzma? that would make it the first game I can remember that's decently compressed

-!- pengvado changed the topic of #x264 to: #x264 - opensource MPEG-4 AVC / H.264 and touhou discussion - for development see #x264dev - http://videolan.org/x264.html

<pengvado> is there some tradition (both in shooters and fighting games) of having no key config options? or have the devs never realized they're not programming for arcade machines anymore?
<pengvado> e.g. IAMP. uses G,Y,J,N to move. fuck you, I'm on qwerty and my hands can't reach all those keys at the same time

<NeonFloss> OBAMA!
<pengvado> off topic!
<NeonFloss> how?
<ChronoCross> just say something like Obama wins......touhou is fun. and you'll be fine, then your on topic
<pengvado> "touhou is fun" is a tautology. as such, it doesn't convey any information and doesn't offset any other sins.

<japhb> Dark_Shikari: BTW, could you post with/without comparisons of frames showing off your MB-tree win?
<Dark_Shikari> japhb: that's difficult because MB-tree is a ratecontrol algorithm, and its most obvious effect is that it redistributes bits among frames.
<Dark_Shikari> so some frames will get worse, and others better.
<Dark_Shikari> so individual frame comparisons generally aren't very useful for judging the overall effect.
<pengvado> so post touhou
On Windows
<pengvado> any game that installs slowly uses cab
<Dark_Shikari> wait, I thought CAB was fast, albeit shit?
<pengvado> cab is slower than lzma, and still shit

<pengvado> and that would be why friends don't let friends write batch scripts
<pengvado> if you ask me that, the answer would begin with "install perl"

<pengvado> that brings back the memory:
<pengvado> windows is posix compliant
<pengvado> every single posix function returns ENOTIMPLEMENTED, which is one complying implementation :)
Miscellaneous
<videowall_> pengvado: you should really try to find someone that measures up to your standards that will help review patches :)
<videowall_> or alternatively, consider clone technology
<pengvado> penguins don't clone, we fork()

<pengvado> Dark_Shikari: so, you want to become the 3rd developer I've ever met in meatspace?
<checkers> Dark_Shikari: find out if the other two devs are still alive before saying yes

<pengvado> http://akuvian.org/src/x264/benchasm_align.diff <-- for various values of 0

<pengvado> <Dark_Shikari> if a fansubber or DVD rip group uploaded an H.264-in-AVI file they'd get laughed off the internet
<Dark_Shikari> Probably
<pengvado> but asp-in-avi wouldn't be laughed at
<Dark_Shikari> of course not, ASP in AVI is normal unless you want softsubs
<pengvado> and asp-in-avi requires exactly the same ugly hacks
<Dark_Shikari> Probably because its been that way so long that everyone is accustomed to it
<pengvado> which just goes to show that our campaign to use a new codec as an excuse to tell people to upgrade their container is working

<jarod> if you go by psyhics, all is meaningless, as in its already planned ahead :0
<pengvado> psyhics = espers from the boonies?

<checkers> <pengvado> checkers: so, graph time? <-- CPU prices never make enough sense
<Dark_Shikari> they're like airfares
<pengvado> sure, the business behind airfares is: charge each customer as much as they'll pay
<pengvado> that's the optimal strategy in any situation you can get away with it

-!- pengvado [n=pengvado@akuvian.org] has joined #x264dev
-!- pengvado1 [n=pengvado@akuvian.org] has joined #x264dev
<holger_> hooray, pengvado just forked ;)

<Dark_Shikari> pengvado: you really don't have to bother yourself with responding to fools
<pengvado> I have a certain amount of flame to go around, and some days I don't find people who deserve it, so I flame the less worthy

<Darkvater> if I'm here I'd like to post another question. When configuring I'm told to install yasm. but I already have it (0.4.0) yet it detection fails? any ideas?
<pengvado> and if you're not here we should ignore the question?

<pengvado> "asleep" is just the common word for "my blood-caffeine level dropped below .2%"

<pengvado> seriously, I can't write anything on paper to save my life
<pengvado> the only reason I graduated college is because I weaseled my way out of every single paper

<jarod> hmm isn;t that a human emotion?
<pengvado> of course. I find great pride in my ability to accurately emulate emotions.

<AnarkiNet> the information i read was recent, within a week or two
<pengvado> people can be wrong a week ago too

<Dark_Shikari> but yeah, people like pengvado have it easy, they don't have income, so they don't have to pay taxes!
<pengvado> something like that. I have enough investments to pay for my 10k/year living expenses...

<Dark_Shikari> somehow quicktime on my system got completely broken.
<Dark_Shikari> guess I have to reinstall it... piece of shit.
<Dark_Shikari> it broke without me even doing anything :/
<pengvado> well duh, quicktime ships pre-broken

<Dark_Shikari> 0-31 is 5 bits
<pengvado> oops, I fail at binary

<iive> so the mystery of heavy toolcase escape would remain unsolved...
<Dopefish> i'll always wonder how they stole it
<iive> of course... there is simpler explanation, that covers all the facts.... and this is that...
<iive> YOU DID IT.
<Dopefish> are you kidding? it took three people to get that toolchest up the hill _without_ all the tools in it
<pengvado> was one of the tools a forklift?

<Rugart> im using by default mp4box -inter 500 for all my videos when i run my encode script
<Rugart> its similar, but what is better?
<Dark_Shikari> what's better, an apple or an orange?
<pengvado> orange, definitely.

<meowcats> at least naruto doesnt have magic talking swords, or bring people back from the dead multiple times :|
* elenril fails to see what's wrong with magic talking swords
<pengvado> if you can bring people back from the dead once, why shouldn't you do it again?
<meowcats> whats the point of having a character die if they can be brought back?
<pengvado> that just means they weren't very dead to begin with.

<pengvado> and japanese people can't say a repeated consonant either, so they have no need to represent it in writing
<Dark_Shikari> Unless it's an n.
<pengvado> n is a vowel
<pengvado> you know it is because it has its own character