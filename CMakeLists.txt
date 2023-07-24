#
# Copyright (c) 2023 Christian Spielberger
#

include(CheckIncludeFile)
include(CheckSymbolExists)
include(CheckFunctionExists)

zephyr_library()

list(APPEND RE_DEFINITIONS -DRE_VERSION="3.3.0")

check_symbol_exists("arc4random" "stdlib.h" HAVE_ARC4RANDOM)
if(HAVE_ARC4RANDOM)
  list(APPEND RE_DEFINITIONS -DHAVE_ARC4RANDOM)
endif()

check_include_file(unistd.h HAVE_UNISTD_H)
if(HAVE_UNISTD_H)
  list(APPEND RE_DEFINITIONS -DHAVE_UNISTD_H)
endif()

if(${CMAKE_SYSTEM_NAME} MATCHES "OpenBSD")
  check_symbol_exists(res_init resolv.h HAVE_RESOLV)
else()
  check_symbol_exists(res_ninit resolv.h HAVE_RESOLV)
endif()
if(HAVE_RESOLV)
  set(RESOLV_LIBRARY resolv)
  list(APPEND RE_DEFINITIONS -DHAVE_RESOLV)
endif()

#check_function_exists(accept4 HAVE_ACCEPT4)
#if(HAVE_ACCEPT4)
#  list(APPEND RE_DEFINITIONS -DHAVE_ACCEPT4)
#endif()

#list(APPEND RE_DEFINITIONS -DHAVE_PTHREAD)
#set(HAVE_PTHREAD ON)

check_symbol_exists(epoll_create "sys/epoll.h" HAVE_EPOLL)
if(HAVE_EPOLL)
   list(APPEND RE_DEFINITIONS -DHAVE_EPOLL)
endif()

list(APPEND RE_DEFINITIONS
  -D_GNU_SOURCE
  -DHAVE_ATOMIC
  -DHAVE_SELECT
  )

list(APPEND RE_DEFINITIONS
  -DHAVE_STRERROR_R
  -DHAVE_STRINGS_H
  -DHAVE_SYS_TIME_H
  -DHAVE_SELECT_H
  )

list(APPEND RE_DEFINITIONS
   -DUSE_MBEDTLS
)

list(APPEND RE_DEFINITIONS
 -DHAVE_UNIXSOCK=1
)

list(APPEND RE_DEFINITIONS
  -DARCH="${CMAKE_SYSTEM_PROCESSOR}"
  -DOS="${CMAKE_SYSTEM_NAME}"
)

list(APPEND RE_DEFINITIONS -DRELEASE)

zephyr_library_compile_definitions(${RE_DEFINITIONS})
zephyr_include_directories(
        ${ZEPHYR_CURRENT_MODULE_DIR}/lib/include
)

zephyr_compile_options(${RE_DEFINITIONS})


zephyr_library_sources(
  lib/src/av1/depack.c
  lib/src/av1/obu.c
  lib/src/av1/pkt.c

  lib/src/async/async.c

  lib/src/base64/b64.c

  lib/src/btrace/btrace.c

  lib/src/conf/conf.c

  lib/src/dbg/dbg.c

  lib/src/dns/client.c
  lib/src/dns/cstr.c
  lib/src/dns/dname.c
  lib/src/dns/hdr.c
  lib/src/dns/ns.c
  lib/src/dns/rr.c
  lib/src/dns/rrlist.c

  lib/src/fmt/ch.c
  lib/src/fmt/hexdump.c
  lib/src/fmt/pl.c
  lib/src/fmt/print.c
  lib/src/fmt/prm.c
  lib/src/fmt/regex.c
  lib/src/fmt/str.c
  lib/src/fmt/str_error.c
  lib/src/fmt/time.c
  lib/src/fmt/unicode.c

  lib/src/h264/getbit.c
  lib/src/h264/nal.c
  lib/src/h264/sps.c

  lib/src/h265/nal.c

  lib/src/hash/func.c
  lib/src/hash/hash.c

  lib/src/hmac/hmac_sha1.c

  lib/src/http/auth.c
  lib/src/http/chunk.c
  lib/src/http/client.c
  lib/src/http/msg.c
  lib/src/http/request.c
  lib/src/http/server.c

  lib/src/httpauth/basic.c
  lib/src/httpauth/digest.c

  lib/src/ice/cand.c
  lib/src/ice/candpair.c
  lib/src/ice/chklist.c
  lib/src/ice/comp.c
  lib/src/ice/connchk.c
  lib/src/ice/icem.c
  lib/src/ice/icesdp.c
  lib/src/ice/icestr.c
  lib/src/ice/stunsrv.c
  lib/src/ice/util.c

  lib/src/json/decode.c
  lib/src/json/decode_odict.c
  lib/src/json/encode.c

  lib/src/list/list.c

  lib/src/main/init.c
  lib/src/main/main.c
  lib/src/main/method.c

  lib/src/mbuf/mbuf.c

  lib/src/md5/wrap.c

  lib/src/mem/mem.c
  lib/src/mem/secure.c

  lib/src/mod/mod.c

  lib/src/mqueue/mqueue.c

  lib/src/msg/ctype.c
  lib/src/msg/param.c

  lib/src/net/if.c
  lib/src/net/net.c
  lib/src/net/netstr.c
  lib/src/net/rt.c
  lib/src/net/sock.c
  lib/src/net/sockopt.c
  lib/src/net/zephyr/zif.c

  lib/src/odict/entry.c
  lib/src/odict/get.c
  lib/src/odict/odict.c
  lib/src/odict/type.c

  lib/src/rtp/fb.c
  lib/src/rtp/member.c
  lib/src/rtp/ntp.c
  lib/src/rtp/pkt.c
  lib/src/rtp/rr.c
  lib/src/rtp/rtcp.c
  lib/src/rtp/rtp.c
  lib/src/rtp/sdes.c
  lib/src/rtp/sess.c
  lib/src/rtp/source.c

  lib/src/rtpext/rtpext.c

  lib/src/sa/printaddr.c
  lib/src/sa/sa.c

  lib/src/sdp/attr.c
  lib/src/sdp/format.c
  lib/src/sdp/media.c
  lib/src/sdp/msg.c
  lib/src/sdp/session.c
  lib/src/sdp/str.c
  lib/src/sdp/util.c

  lib/src/sha/wrap.c

  lib/src/shim/shim.c

  lib/src/srtp/misc.c
  lib/src/srtp/replay.c
  lib/src/srtp/srtcp.c
  lib/src/srtp/srtp.c
  lib/src/srtp/stream.c

  lib/src/stun/addr.c
  lib/src/stun/attr.c
  lib/src/stun/ctrans.c
  lib/src/stun/dnsdisc.c
  lib/src/stun/hdr.c
  lib/src/stun/ind.c
  lib/src/stun/keepalive.c
  lib/src/stun/msg.c
  lib/src/stun/rep.c
  lib/src/stun/req.c
  lib/src/stun/stun.c
  lib/src/stun/stunstr.c

  lib/src/sys/daemon.c
  lib/src/sys/endian.c
  lib/src/sys/fs.c
  lib/src/sys/rand.c
  lib/src/sys/sleep.c
  lib/src/sys/sys.c

  lib/src/tcp/tcp.c
  lib/src/tcp/tcp_high.c

  lib/src/telev/telev.c

  lib/src/thread/thread.c

  lib/src/tmr/tmr.c

  lib/src/trace/trace.c

  lib/src/trice/cand.c
  lib/src/trice/candpair.c
  lib/src/trice/chklist.c
  lib/src/trice/connchk.c
  lib/src/trice/lcand.c
  lib/src/trice/rcand.c
  lib/src/trice/stunsrv.c
  lib/src/trice/tcpconn.c
  lib/src/trice/trice.c

  lib/src/turn/chan.c
  lib/src/turn/perm.c
  lib/src/turn/turnc.c

  lib/src/udp/udp.c

  lib/src/uri/uri.c
  lib/src/uri/uric.c

  lib/src/websock/websock.c

  lib/rem/aac/aac.c
  lib/rem/au/fmt.c
  lib/rem/au/util.c
  lib/rem/aubuf/aubuf.c
  lib/rem/aubuf/ajb.c
  lib/rem/auconv/auconv.c
  lib/rem/aufile/aufile.c
  lib/rem/aufile/wave.c
  lib/rem/auframe/auframe.c
  lib/rem/aulevel/aulevel.c
  lib/rem/aumix/aumix.c
  lib/rem/auresamp/resamp.c
  lib/rem/autone/tone.c
  lib/rem/avc/config.c
  lib/rem/dtmf/dec.c
  lib/rem/fir/fir.c
  lib/rem/g711/g711.c
  lib/rem/goertzel/goertzel.c
  lib/rem/vid/draw.c
  lib/rem/vid/fmt.c
  lib/rem/vid/frame.c
  lib/rem/vidconv/vconv.c
  lib/rem/vidmix/vidmix.c

  lib/src/jbuf/jbuf.c

  lib/src/sip/addr.c
  lib/src/sip/auth.c
  lib/src/sip/contact.c
  lib/src/sip/cseq.c
  lib/src/sip/ctrans.c
  lib/src/sip/dialog.c
  lib/src/sip/keepalive.c
  lib/src/sip/keepalive_udp.c
  lib/src/sip/msg.c
  lib/src/sip/rack.c
  lib/src/sip/reply.c
  lib/src/sip/request.c
  lib/src/sip/sip.c
  lib/src/sip/strans.c
  lib/src/sip/transp.c
  lib/src/sip/via.c

  lib/src/sipevent/listen.c
  lib/src/sipevent/msg.c
  lib/src/sipevent/notify.c
  lib/src/sipevent/subscribe.c

  lib/src/sipreg/reg.c

  lib/src/sipsess/accept.c
  lib/src/sipsess/ack.c
  lib/src/sipsess/close.c
  lib/src/sipsess/connect.c
  lib/src/sipsess/info.c
  lib/src/sipsess/listen.c
  lib/src/sipsess/modify.c
  lib/src/sipsess/prack.c
  lib/src/sipsess/reply.c
  lib/src/sipsess/request.c
  lib/src/sipsess/sess.c
  lib/src/sipsess/update.c

  lib/src/aes/stub.c
  lib/src/hmac/hmac.c
  lib/src/tls/stub.c

  lib/src/crc32/crc32.c

  lib/src/thread/posix.c

)

