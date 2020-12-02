# Netalo Core Chat

## Overview of Netalo Core Chat

Netalo Core Chat provides socketio-based communications for external interactions. All socketio messages have json format.

The socketio url: 
&nbsp;&nbsp;&nbsp;&nbsp;http://hostname:port/data?session=abcdef,
where 'abcdef' is session string which is obtained during login process

Example:
```
  var socket = io('ws://example:2082/data?session=abcdef',
       {
         transports: ['websocket'],
         extraHeaders: {Session: 'abcdef'}
       });
  socket.on("login", (result) => {
    //handle login result
    if (result.result == 0) {
        //success
    } else {
        //error
        if (result.result == 2) {
            //request new session
        }
    }
  });
  socket.on("user_status", (status) => {
    //updating friend's online status
  });
  socket.emit("list_group", "{'uin': 123, 'pindex': 0, 'psize': 10, 'sort_by': 1}", (result) => {
    // handle groups if success
  });
```
|SocketIO event     | Description                        
|----------------|-------------------------------
|register_profile_token|register device token which is used for notification
|unregister_profile_token|unregister device token which is used for notification
|create_group|create a chat group
|update_group|update a chat group
|leave_group|leave a chat group
|delete_conversation|delete conversation in a chat group
|list_group|get a list of chat groups of a user
|block_user|block a user in chat group
|unblock_user|unblock a user in chat group
|check_group|check whether a chat group exists
|mute_group|turn off chat group notificaiton
|unmute_group|turn on chat group notification
|pin_group|pin a chat group
|unpin_group|unpin a chat group
|create_message|send a message in a chat group
|update_message|update a message in a chat group
|delete_message|delete a message in a chat group
|list_message|get list of messages in a chat group
|create_call|start audio/video call
|answer_call|answer an incoming audio/video call
|stop_call|hang an audio/video call
|call_status|check status of an audio/video call
|ice_sdp|send webrtc sdp information
|ice_candidate|send webrtc media endpoints
|login|trigger when server finishes authentication process
|event|an user event such as typing, call events, etc
|user_status|trigger when a friend's online status is changed

## Netalo API Reference

##### Register profile token

|Request Parameter | Type | Description                        
|----------------|----------|---------------------
|uin| uint64 | User Identification number
|type| enum | The token platform type
|token| string | The device token

|Response Parameter | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code
|uin| uint64 | User Identification number
|type| enum | The token platform type
|token| string | The device token

The enum 'type' has one of following values:
 1: Adnroid token device
 2: IOS token device
 3: Web token device
 4: IOS pushkit token device

The error code value of 0 means success, otherwise error occurs.
Example
```
socket.emit("register_profile_token", "{'uin': 123, 'type': 1, 'token': 'abc'}", (result) => {
    //handle result
});
```

##### Unregister profile token

|Request Parameter     | Type | Description                        
|----------------|----------|---------------------
|uin| uint64 | User Identification number
|token| string | The device token

|Response Parameter | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code
|uin| uint64 | User Identification number
|token| string | The device token

The error code value of 0 means success, otherwise error occurs.
Example:
```
socket.emit("unregister_profile_token", "{'uin': 123, 'type': 1, 'token': 'abc'}", (result) => {
    //handle result
});
```

##### Create Group

|Request Parameter     | Type | Description                        
|----------------|----------|---------------------
|appid| uint64 | Application Identification number
|name| string | The group name
|type| enum | The group type
|avatar_url| string | The group avatar url
|owner_uin| uint64 | The group owner's user identificaiton number
|occupants_uins| array of uint64 | List of group member's user identiftion numbers

The enum 'type' has one of following values:
 1: private group
 2: community group
 3: public group

|Response Parameter | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code
|group| Group | The group object

|Group Memeber | Type | Description                        
|----------------|----------|---------------------
|group_id| uint64 | The group identification number
|type| enum | The group type
|name| string | The group name
|avatar_url| string | The group avatar url
|owner_uin| uint64 | The group owner's user identificaiton number
|creator_uin| uint64 | The group creator's user identificaiton number
|occupants_uins| array of uint64 | List of group member's user identiftion numbers
|created_at| uint64 | The time of creating group
|updated_at| uint64 | The last time of updating group
|last_message_id| uint64 | The last message id of group

The error code value of 0 means success, otherwise error occurs.
Example:
```
socket.emit("create_group", "{'appid': 123, 'name': "Netalo", 'type': 2, 'avatar_url': 'some url', 'owner_uin': 123, 'occupants_uin': [123, 456]}", (result) => {
    //handle result
});
```

##### Update Group

|Request Parameter     | Type | Description                        
|----------------|----------|---------------------
|group_id| uint64 | The group identification number
|name| string | The group name
|owner_uin| uint64 | The group owner's user identificaiton number
|type| enum | The group type
|avatar_url| string | The group avatar url

|Response Parameter | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code
|group| Group | The group object

The error code value of 0 means success, otherwise error occurs.
Example:
```
socket.emit("create_group", "{'group_id': 123, 'name': "Netalo", 'avatar_url': 'some url', 'owner_uin': 123, 'push_all': [123, 456], 'pull_all': [789, 102]}}", (result) => {
    //handle result
});
```

##### Leave Group

|Request Parameter     | Type | Description                        
|----------------|----------|---------------------
|group_id| uint64 | The group identification number

|Response Parameter | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code

The error code value of 0 means success, otherwise error occurs.
Example:
```
socket.emit("leave_group", "{'group_id': 123}", (result) => {
    //handle result
});
```

##### Delete Conversation

|Request Parameter     | Type | Description                        
|----------------|----------|---------------------
|group_id| uint64 | The group identification number

|Response Parameter | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code

The error code value of 0 means success, otherwise error occurs.
Example:
```
socket.emit("delete_conversation", "{'group_id': 123}", (result) => {
    //handle result
});
```

##### List Group

|Request Parameter     | Type | Description                        
|----------------|----------|---------------------
|uin| uint64 | User identification number
|pindex| uint32 | The page index
|psize| uint32 | The page size
|sort_by| enum | The page size

The enum 'sort_by' has one of following values:
 1: sort by last message
 2: sort by creation time

|Response Parameter | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code
|groups| array of Groups | The list of groups

The error code value of 0 means success, otherwise error occurs.
Example:
```
socket.emit("list_group", "{'uin': 123, 'pindex': 0, 'psize': 10, 'sort_by': 1}", (result) => {
    //handle result
});
```

##### Block user

|Request Parameter     | Type | Description                        
|----------------|----------|---------------------
|group_id| uint64 | The group identification number
|blocked_uin| uint64 | User Identification number

|Response Parameter | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code

The error code value of 0 means success, otherwise error occurs.
Example:
```
socket.emit("block_user", "{'group_id': 123, 'blocked_uin': 456}", (result) => {
    //handle result
});
```

##### Unblock user

|Request Parameter     | Type | Description                        
|----------------|----------|---------------------
|group_id| uint64 | The group identification number
|unblocked_uin| uint64 | User Identification number

|Response Parameter | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code

The error code value of 0 means success, otherwise error occurs.
Example:
```
socket.emit("unblock_user", "{'group_id': 123, 'blocked_uin': 456}", (result) => {
    //handle result
});
```

##### Check Group

|Request Parameter| Type | Description                        
|----------------|----------|---------------------
|type| enum | The group type
|occupants_uins| array of uint64 | List of group member's user identiftion numbers

Only 'type' of private and public group is valid for this request.

|Response Parameter | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code
|group| Group | The group object

The error code value of 0 means success, otherwise error occurs.
Example:
```
socket.emit("check_group", "{'type': 1, 'occupants_uin': [123, 456]}", (result) => {
    //handle result
});
```

##### Mute Group

|Request Parameter     | Type | Description                        
|----------------|----------|---------------------
|group_id| uint64 | The group identification number

|Response Parameter | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code

The error code value of 0 means success, otherwise error occurs.
Example:
```
socket.emit("mute_group", "{'group_id': 123}", (result) => {
    //handle result
});
```

##### Unmute Group

|Request Parameter     | Type | Description                        
|----------------|----------|---------------------
|group_id| uint64 | The group identification number

|Response Parameter | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code

The error code value of 0 means success, otherwise error occurs.
Example:
```
socket.emit("unmute_group", "{'group_id': 123}", (result) => {
    //handle result
});
```

##### Pin Group

|Request Parameter     | Type | Description                        
|----------------|----------|---------------------
|group_id| uint64 | The group identification number

|Response Parameter | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code

The error code value of 0 means success, otherwise error occurs.
Example:
```
socket.emit("pin_group", "{'group_id': 123}", (result) => {
    //handle result
});
```

##### Unpin Group

|Request Parameter     | Type | Description                        
|----------------|----------|---------------------
|group_id| uint64 | The group identification number

|Response Parameter | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code

The error code value of 0 means success, otherwise error occurs.
Example:
```
socket.emit("unpin_group", "{'group_id': 123}", (result) => {
    //handle result
});
```

##### Create Message

|Request Parameter     | Type | Description                        
|----------------|----------|---------------------
|group_id| uint64 | The group identification number
|type| enum | The message type
|message| string | The content of message
|attactments| string | Custom content of message
|group_name| string | The group name
|group_avatar| string | The group avatar url
|sender_name| string | The sender name
|send_avatar| string | The sender avatar url
|nonce| string | A random string to differeciate messages
|version| uint32 | The version of message

|Response Parameter | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code
|message| Message | The message object

|Message member | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code
|message_id| uint64 | The message id
|group_id| uint64 | The group identification number
|type| enum | The message type
|message| string | The content of message
|attactments| string | Custom content of message
|group_name| string | The group name
|group_avatar| string | The group avatar url
|sender_name| string | The sender name
|send_avatar| string | The sender avatar url
|created_at| uint64 | The time of creating message

The error code value of 0 means success, otherwise error occurs.
Example:
```
socket.emit("create_message", "{'group_id': 123, 'type': 1, 'message': 'abc', 'nonce': 'random', 'version': 0}", (result) => {
    //handle result
});
```

##### Update Message

|Request Parameter     | Type | Description                        
|----------------|----------|---------------------
|message_id| uint64 | The message id
|group_id| uint64 | The group identification number
|message| string | The new content of message

|Response Parameter | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code
|message| Message | The message object

The error code value of 0 means success, otherwise error occurs.
Example:
```
socket.emit("update_message", "{'group_id': 123, 'message_id': 456, 'message': 'abc'}", (result) => {
    //handle result
});
```

##### Delete Message

|Request Parameter     | Type | Description                        
|----------------|----------|---------------------
|message_id| uint64 | The message id
|group_id| uint64 | The group identification number

|Response Parameter | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code

The error code value of 0 means success, otherwise error occurs.
Example:
```
socket.emit("delete_message", "{'group_id': 123, 'message_id': 456}", (result) => {
    //handle result
});
```

##### List Message

|Request Parameter     | Type | Description                        
|----------------|----------|---------------------
|group_id| uint64 | The group identification number
|last_message_id| uint64 | All returned messages must be created before the last message
|last_seen_message_id| uint64 | All returned messages must be created after the last seen message
|pindex| uint32 | The page index
|psize| uint32 | The page size

|Response Parameter | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code
|messages| Message | The device token

The error code value of 0 means success, otherwise error occurs.
Example:
```
socket.emit("list_message", "{'group_id': 123, 'last_message_id': 1598157020, 'last_seen_message_id': 1598151020'}", (result) => {
    //handle result
});
```

##### Create Call

|Request Parameter     | Type | Description                        
|----------------|----------|---------------------
|media_type| enum | Media call type
|group_id| uint64 | The group identification number
|caller_uin| uint64 | Caller Identification number
|callee_uins| uint64 | The list of callee Identification numbers
|group_name| string | The group name
|group_avatar| string | The group avatar url
|sender_name| string | The sender name
|send_avatar| string | The sender avatar url

The enum 'media_type' has one of following values:
 1: audio call
 2: video call

|Response Parameter | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code
|call| Call | The Call object

|Call member     | Type | Description                        
|----------------|----------|---------------------
|call_id| uint64 | The call identification number
|group_id| uint64 | The group identification number
|media_type| enum | Media call type
|call_status| enum | The status of call
|caller_uin| uint64 | Caller Identification number
|callee_uins| array of uint64 | The list of callee Identification numbers
|group_name| string | The group name
|group_avatar| string | The group avatar url
|sender_name| string | The sender name
|send_avatar| string | The sender avatar url
|started_at| uint64 | Time of starting call
|accepted_at| uint64 | Time of call acceptance
|stopped_at| uint64 | Time of stopping call

The enum 'call_status' has one of following values:
 1: ongoing call
 2: timeout call
 3: finished call
 
The error code value of 0 means success, otherwise error occurs.
Example:
```
socket.emit("create_call", "{'media_type': 2, 'group_id': 123, 'caller_uin': 456, 'callee_uins': [123,456]}", (result) => {
    //handle result
});
```

##### Answer Call

|Event Parameter     | Type | Description                        
|----------------|----------|---------------------
|answer| enum | The call answer
|call_id| uint64 | The call identification number
|group_id| uint64 | The group identification number
|uin| uint64 | User Identification number

The enum 'answer' has one of following values:
 1: accept
 2: reject
 3: timeout
 4: failed
 
Example:
```
socket.emit("answer_call", "{'call_id': 123, 'group_id': 456, 'uin': 789}");
```

##### Stop Call

|Event Parameter     | Type | Description                        
|----------------|----------|---------------------
|answer| enum | The call answer
|call_id| uint64 | The call identification number
|group_id| uint64 | The group identification number
|uin| uint64 | User Identification number

Example:
```
socket.emit("stop_call", "{'call_id': 123, 'group_id': 456, 'uin': 789}");
```

##### Call Status

|Request Parameter     | Type | Description                        
|----------------|----------|---------------------
|call_id| uint64 | The call identification number
|group_id| uint64 | The group identification number

|Response Parameter | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code
|call_id| uint64 | The call identification number
|group_id| uint64 | The group identification number
|call_status| enum | The status of call

The enum 'status' has one of following values:
 1: ongoing call
 2: timeout call
 3: finished call
 
The error code value of 0 means success, otherwise error occurs.
Example:
```
socket.emit("call_status", "{'call_id': 123, 'group_id': 1}", (result) => {
    //handle result
});
```

##### ICE SDP 

|Event Parameter     | Type | Description                        
|----------------|----------|---------------------
|call_id| uint64 | The call identification number
|group_id| uint64 | The group identification number
|sender_uin| uint64 | Sender Identification number
|receiver_uin| uint64 | Receiver Identification number
|type| enum | Type of sdp information
|sdp| string | Content of sdp

The enum 'type' has one of following values:
 1: offer
 2: answer

Example:
```
socket.emit("ice_sdp", "{'call_id': 123, 'group_id': 456, 'sender_uin': 789, 'receiver_uin': 123, 'type': 1, 'sdp': 'sdp'}");
```

##### Ice candidate

|Request Parameter     | Type | Description                        
|----------------|----------|---------------------
|call_id| uint64 | The call identification number
|group_id| uint64 | The group identification number
|sender_uin| uint64 | Sender Identification number
|receiver_uin| uint64 | Receiver Identification number
|candidate| string | Candidate information
|sdp_mid| string | The media stream identification
|sdp_mline_index| uint32 | The media stream line index

Example:
```
socket.emit("ice_candidate", "{'call_id': 123, 'group_id': 456, 'sender_uin': 789, 'receiver_uin': 123, 'type': 1, 'candidate': 'candidate', 'sdp_mid': '0', 'sdp_mline_index': 0}");
```

##### Login

|Event Parameter     | Type | Description                        
|----------------|----------|---------------------
|result| uint64 | Error code

The error code value:
0: success
2: expired session
7: conflict login

Example:
```
socket.on("login", (result) => {
    //handle result
});
```

##### Event

|Event Parameter     | Type | Description                        
|----------------|----------|---------------------
|type| enum | The event type
|sender_uin| uint64 | Sender Identification number
|receiver_uin| uint64 | Receiver Identification number
|data| string | Custom data
|call_id| uint64 | The call identification number (optional)
|group_id| uint64 | The group identification number (optional)

The enum 'type' has one of following values:
 1: cancelling call
 2: reset call
 3: call received event
 4: request video call
 5: deactivate video call
 6: activate video call
 7: check call availability
 8: start typing
 9: stop typing
 
Example:
```
socket.on("event", (ev) => {
    //handle event
});
```

##### User Status

|Request Parameter     | Type | Description                        
|----------------|----------|---------------------
|uin| uint64 | User Identification number
|online_status| enum | User online status
|message_status| string | The string status

The enum 'online_status' has one of following values:
 1: online
 2: offline
 
Example:
```
socket.on("user_status", (status) => {
    //handle user status
});
```
