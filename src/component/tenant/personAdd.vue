<template>
	<v-main style="margin:0 10px;">
		<div style="height:10px;"></div>
		<v-card>
			<v-card-title>
				<span class="headline">Add</span>
				<v-spacer></v-spacer>
				<v-btn fab small elevation="0" @click="$router.push('/tenant/person')">
					<v-icon>mdi-close</v-icon>
				</v-btn>
			</v-card-title>
			<v-card-text>
				<v-text-field v-model="person.name" label="Name"></v-text-field>
				<v-text-field v-model="person.email" label="Email"></v-text-field>
				<v-text-field v-model="person.password" label="Password" type="password"></v-text-field>
				<v-switch v-model="person.active" label="Active"></v-switch>
				<v-select v-model="person.roleSet" :items="roleList" label="Role" multiple chips item-value="value" item-text="text"></v-select>
				<v-row>
					<v-col md="2" cols="12">
						<v-file-input v-model="attachment1" label="Attachment"></v-file-input>
					</v-col>
					<v-col md="2" cols="12">
						<v-file-input v-model="attachment2" label="Attachment"></v-file-input>
					</v-col>
					<v-col md="2" cols="12">
						<v-file-input v-model="attachment3" label="Attachment"></v-file-input>
					</v-col>
					<v-col md="2" cols="12">
						<v-file-input v-model="attachment4" label="Attachment"></v-file-input>
					</v-col>
					<v-col md="2" cols="12">
						<v-file-input v-model="attachment5" label="Attachment"></v-file-input>
					</v-col>
					<v-col md="2" cols="12">
						<v-file-input v-model="attachment6" label="Attachment"></v-file-input>
					</v-col>
				</v-row>
			</v-card-text>
			<v-card-actions>
				<v-spacer></v-spacer>
				<v-btn @click="$router.push('/tenant/person')">Cancel</v-btn>
				<v-btn @click="addPerson" color="primary">Add</v-btn>
			</v-card-actions>
		</v-card>
	</v-main>
</template>

<script>
/* eslint-disable no-unused-vars */
import axios from "axios";

export default {
	data: function() {
		return {
			person: {},
			attachment1: null,
			attachment2: null,
			attachment3: null,
			attachment4: null,
			attachment5: null,
			attachment6: null,
			roleList: []
		};
	},
	mounted: function() {
		this.listRole();
	},
	methods: {
		listRole() {
			axios
				.get("/system/common/person/role/list")
				.then(response => {
					this.roleList = response.data;
				})
				.catch(() => {});
		},
		addPerson() {
			let formData = new FormData();
			formData.append("person", JSON.stringify(this.person));
			if (this.attachment1) {
				formData.append("attachment1", this.attachment1);
			}
			if (this.attachment2) {
				formData.append("attachment2", this.attachment2);
			}
			if (this.attachment3) {
				formData.append("attachment3", this.attachment3);
			}
			if (this.attachment4) {
				formData.append("attachment4", this.attachment4);
			}
			if (this.attachment5) {
				formData.append("attachment5", this.attachment5);
			}
			if (this.attachment6) {
				formData.append("attachment6", this.attachment6);
			}
			axios
				.post("/system/person/add", formData, {
					headers: {
						"Content-Type": "multipart/form-data"
					}
				})
				.then(() => {
					this.$router.push("/tenant/person");
				})
				.catch(() => {});
		}
	}
};
</script>