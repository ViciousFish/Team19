<template>
  <div class="flex-auto message p2">
    <div class="flex flex-auto items-center" v-if="!editing && message!==null">
      <p class="mr1">{{prettyDate}}</p>
      <p class="my0 flex-auto">{{message.text}}</p>
      <a v-if="editable" @click="edit">edit</a>
    </div>
    <a @click="edit" v-if="!editing && message===null">+ new</a>
    <form @submit="save" class="flex flex-auto" v-if="editing">
      <label class="ml1 mr1" for="date">Date</label>
      <datepicker name="date"
        v-model="temp.date"
        input-class="p1 mr1"
        placeholder="date to send"
        :disabled-dates="disabledDates"
        :highlighted="highlighted"
        ></datepicker>
      <label class="ml1 mr1" for="hour">Hour</label>
      <select v-model="hour" name="hour">
        <option value="0" selected>00</option>
        <option value="1">01</option>
        <option value="2">02</option>
        <option value="3">03</option>
        <option value="4">04</option>
        <option value="5">05</option>
        <option value="6">06</option>
        <option value="7">07</option>
        <option value="8">08</option>
        <option value="9">09</option>
        <option value="10">10</option>
        <option value="11">11</option>
        <option value="12">12</option>
        <option value="13">13</option>
        <option value="14">14</option>
        <option value="15">15</option>
        <option value="16">16</option>
        <option value="17">17</option>
        <option value="18">18</option>
        <option value="19">19</option>
        <option value="20">20</option>
        <option value="21">21</option>
        <option value="22">22</option>
        <option value="23">23</option>
      </select>
      <label class="ml1 mr1" for="minute">Minute</label>
      <select v-model="minute" name="minute">
        <option value="0" selected>00</option>
        <option value="1">01</option>
        <option value="2">02</option>
        <option value="3">03</option>
        <option value="4">04</option>
        <option value="5">05</option>
        <option value="6">06</option>
        <option value="7">07</option>
        <option value="8">08</option>
        <option value="9">09</option>
        <option value="10">10</option>
        <option value="11">11</option>
        <option value="12">12</option>
        <option value="13">13</option>
        <option value="14">14</option>
        <option value="15">15</option>
        <option value="16">16</option>
        <option value="17">17</option>
        <option value="18">18</option>
        <option value="19">19</option>
        <option value="20">20</option>
        <option value="21">21</option>
        <option value="22">22</option>
        <option value="23">23</option>
        <option value="24">24</option>
        <option value="25">25</option>
        <option value="26">26</option>
        <option value="27">27</option>
        <option value="28">28</option>
        <option value="29">29</option>
        <option value="30">30</option>
        <option value="31">31</option>
        <option value="32">32</option>
        <option value="33">33</option>
        <option value="34">34</option>
        <option value="35">35</option>
        <option value="36">36</option>
        <option value="37">37</option>
        <option value="38">38</option>
        <option value="39">39</option>
        <option value="40">40</option>
        <option value="41">41</option>
        <option value="42">42</option>
        <option value="43">43</option>
        <option value="44">44</option>
        <option value="45">45</option>
        <option value="46">46</option>
        <option value="47">47</option>
        <option value="48">48</option>
        <option value="49">49</option>
        <option value="50">50</option>
        <option value="51">51</option>
        <option value="52">52</option>
        <option value="53">53</option>
        <option value="54">54</option>
        <option value="55">55</option>
        <option value="56">56</option>
        <option value="57">57</option>
        <option value="58">58</option>
        <option value="59">59</option>
      </select>
      <input class="flex-auto mr1 p1"
        type="text"
        v-model="temp.text"
        placeholder="message text" />
      <input type="submit" class="p1 button" value="save" />
    </form>
  </div>
</template>

<script>
import datepicker from "vuejs-datepicker";

export default {
  props: ["message", "campaignid", "editable"],
  data() {
    return {
      editing: false,
      minuteplus: false,
      temp: {
        uuid: '',
        text: '',
        date: null,
      },
      hour: "0",
      minute: "0",
      highlighted: null,
      disabledDates: null
    };
  },
  computed: {
    prettyDate() {
      const d = new Date(this.message.date);
      // return "" + d.toLocaleString() + d.toTimeString();
      return d.toLocaleString();
    }
  },
  mounted() {
    if (this.message) {
      this.temp.uuid = this.message.uuid;
      this.temp.text = this.message.text;
      this.temp.date = new Date(this.message.date);
    }

    let yesterday = new Date();
    yesterday.setDate(new Date().getDate() - 1);

    this.highlighted = {
      dates: [new Date()]
    };

    this.disabledDates = {
      to: yesterday
    };
  },
  methods: {
    edit() {
      this.editing = true;
    },
    save(e) {
      e.preventDefault();
      if (this.temp.text.length > 0 && this.temp.date !== null) {
        this.editing = false;
        if (!this.message) {
          this.temp.date.setHours(this.hour, this.minute);
          this.$store.dispatch("newMessage", {
            message: this.temp,
            campaign: this.campaignid
          });
          this.temp = {
            uuid: '',
            text: '',
            date: null,
          };
        } else {
          this.temp.date.setHours(this.hour, this.minute);
          this.$store.dispatch('updateMessage', {
            campaignId: this.campaignid,
            message: this.temp
          });
          this.temp = {
            uuid: '',
            text: '',
            date: null,
          };
        }
      }
    }
  },
  components: {
    datepicker
  }
};
</script>

<style scoped>
input {
  /* padding: 15px; */
}
.message {
  background: #fff;
  border-bottom: 1px solid #999;
}
.message a {
  text-decoration: underline;
  cursor: pointer;
}
.message a:hover {
  color: #666;
}
.button {
  /* flex-basis: 100px; */
  flex: 0 0 auto !important;
  /* width: 100px; */
}
.current-date {
  color: #00aaff;
}
</style>
