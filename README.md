# business-assistant
elif menu=="Meetings":
    st.header("📅 Meetings")
    
    # Input for new meeting
    m_title = st.text_input("Meeting title")
    m_link = st.text_input("Link (optional)")
    m_time = st.time_input("Time", datetime.now().time())

    if "meetings" not in st.session_state:
        st.session_state["meetings"] = []

    meetings = st.session_state["meetings"]

    # Add meeting and send Telegram reminder immediately
    if st.button("Add Meeting"):
        if m_title.strip():
            meeting_data = {
                "title": m_title.strip(),
                "link": m_link.strip(),
                "time": str(m_time)
            }
            meetings.append(meeting_data)
            st.session_state["meetings"] = meetings
            st.success("Meeting added.")

            # Send Telegram message immediately
            msg = f"⏰ New Meeting Added:\n- {meeting_data['time']} | {meeting_data['title']}"
            if meeting_data.get("link"):
                msg += f" [Join]({meeting_data['link']})"

            ok = send_telegram_message(msg)
            if ok:
                st.success("Meeting reminder sent via Telegram ✅")
            else:
                st.error("Failed to send Telegram reminder ❌")
        else:
            st.warning("Meeting title required")
