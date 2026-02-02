import React, { useState } from "react";
import "./RegistrationForm.css";

export default function RegistrationForm({ onSubmit }) {
    const [form, setForm] = useState({
        firstName: "",
        email: "",
        password: "",
    });
    const [errors, setErrors] = useState({});
    const [success, setSuccess] = useState("");
    const [submitted, setSubmitted] = useState(null);

    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

    const validate = () => {
        const e = {};
        if (!form.firstName.trim()) e.firstName = "First name is required";
        if (!emailRegex.test(form.email)) e.email = "Valid email is required";
        if (form.password.length < 6) e.password = "Password must be at least 6 characters";
        return e;
    };

    const handleChange = (e) => {
        const { name, value } = e.target;
        setForm((s) => ({ ...s, [name]: value }));
        setErrors((prev) => ({ ...prev, [name]: undefined }));
        setSuccess("");
    };

    const handleSubmit = (e) => {
        e.preventDefault();
        const eObj = validate();
        if (Object.keys(eObj).length) {
            setErrors(eObj);
            setSuccess("");
            return;
        }
        if (typeof onSubmit === "function") onSubmit({ ...form });
        setSubmitted({ ...form });
        setSuccess("Registration successful");
        setForm({
            firstName: "",
            email: "",
            password: "",
        });
        setErrors({});
    };

    const handleData = (e) => {
        e.preventDefault();
        // If all fields are empty, show alert
        const isEmpty = !form.firstName && !form.email && !form.password;
        if (isEmpty) {
            alert("Please enter data before showing it.");
            return;
        }
        // Show the current form data in the submitted box
        setSubmitted({ ...form });
    };

    const field = (label, name, type = "text") => (
        <div className="field-group">
            <label className="field-label">{label}</label>
            <input
                className={`input-field ${errors[name] ? 'error' : ''}`}
                name={name}
                type={type}
                value={form[name]}
                onChange={handleChange}
            />
            {errors[name] && <div className="error-message">{errors[name]}</div>}
        </div>
    );

    return (
        <form onSubmit={handleSubmit} className="registration-form">
            <h2 className="form-title">Register</h2>
            <h3 className="create-account-heading">Create account</h3>
            {field("First Name", "firstName")}
            {field("Email", "email", "email")}
            {field("Password", "password", "password")}
            <button type="submit" className="submit-button">
                Submit
            </button>
            <button type="button" className="submit-button" onClick={handleData} style={{marginTop:8, background:'#4a90e2'}}>
                Show Data
            </button>
            {success && <div className="success-message">{success}</div>}
            {submitted && (
                <div className="submitted-box">
                    <h3 className="submitted-heading">Register</h3>
                    <p>First Name: {submitted.firstName}</p>
                    <p>Email: {submitted.email}</p>
                    <p>Password: {submitted.password}</p>
                </div>
            )}
        </form>
    );
}
