<?php
// PHP Configuration for Form Handling
$notification = '';
$notificationClass = '';

if ($_SERVER['REQUEST_METHOD'] === 'POST' && isset($_POST['submit_contact'])) {
    // Process form submission
    $errors = [];
    $name = filter_input(INPUT_POST, 'name', FILTER_SANITIZE_STRING);
    $email = filter_input(INPUT_POST, 'email', FILTER_VALIDATE_EMAIL);
    $subject = filter_input(INPUT_POST, 'subject', FILTER_SANITIZE_STRING);
    $message = filter_input(INPUT_POST, 'message', FILTER_SANITIZE_STRING);

    // Validation
    if (empty($name)) $errors['name'] = 'Nama lengkap wajib diisi';
    if (!$email) $errors['email'] = 'Email tidak valid';
    if (empty($subject)) $errors['subject'] = 'Subjek wajib diisi';
    if (empty($message)) $errors['message'] = 'Pesan wajib diisi';

    if (empty($errors)) {
        // Email configuration
        $to = 'osisfibbis1@gmail.com';
        $headers = "From: $email\r\n";
        $headers .= "Reply-To: $email\r\n";
        $headers .= "Content-Type: text/plain; charset=UTF-8\r\n";
        
        $email_subject = "Pesan dari Website OSIS: $subject";
        $email_body = "Anda menerima pesan baru dari website OSIS SMA FIBBIS.\n\n";
        $email_body .= "Nama: $name\n";
        $email_body .= "Email: $email\n\n";
        $email_body .= "Pesan:\n$message\n";

        if (mail($to, $email_subject, $email_body, $headers)) {
            $notification = 'Pesan berhasil dikirim!';
            $notificationClass = 'show success';
        } else {
            $notification = 'Gagal mengirim pesan. Silakan coba lagi.';
            $notificationClass = 'show error';
        }
    }
}
?>
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>OSIS SMA FIBBIS 2024/2025 | Vizienfor Generation</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://unpkg.com/aos@2.3.1/dist/aos.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/lightbox2/2.11.4/css/lightbox.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --navy: #001f3f;
            --navy-dark: #001a35;
            --navy-light: #003366;
            --gold: #e6b800;
            --gold-light: #ffd700;
            --gold-dark: #cc9900;
            --white: #ffffff;
            --gray-light: #f8f9fa;
            --gray: #e9ecef;
            --transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
            --shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            --shadow-hover: 0 10px 15px rgba(0, 0, 0, 0.2);
            --gradient: linear-gradient(135deg, var(--navy) 0%, var(--navy-light) 100%);
            --gold-gradient: linear-gradient(135deg, var(--gold) 0%, var(--gold-light) 100%);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Poppins', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        html {
            scroll-behavior: smooth;
        }

        body {
            background-color: var(--white);
            color: var(--navy);
            line-height: 1.7;
            overflow-x: hidden;
            position: relative;
        }

        /* Floating Particles Background */
        .particles {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            pointer-events: none;
        }

        /* Preloader */
        .preloader {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--gradient);
            z-index: 9999;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            transition: opacity 0.5s ease-out;
        }

        .loader {
            width: 60px;
            height: 60px;
            border: 5px solid var(--gold);
            border-radius: 50%;
            border-top-color: transparent;
            animation: rotation 1s linear infinite;
            margin-bottom: 20px;
            position: relative;
        }

        .loader::after {
            content: '';
            position: absolute;
            top: -8px;
            left: -8px;
            right: -8px;
            bottom: -8px;
            border: 5px solid rgba(230, 184, 0, 0.2);
            border-radius: 50%;
            border-top-color: transparent;
            animation: rotation 1.5s linear infinite reverse;
        }

        .preloader-text {
            color: var(--white);
            font-size: 1.2rem;
            letter-spacing: 1px;
            animation: pulse 1.5s infinite;
            text-transform: uppercase;
            font-weight: 300;
            position: relative;
        }

        .preloader-text::after {
            content: '';
            position: absolute;
            bottom: -10px;
            left: 50%;
            transform: translateX(-50%);
            width: 50px;
            height: 2px;
            background: var(--gold);
            animation: expand 2s infinite;
        }

        @keyframes rotation {
            0% { transform: rotate(0deg) }
            100% { transform: rotate(360deg) }
        }

        @keyframes pulse {
            0%, 100% { opacity: 0.6; }
            50% { opacity: 1; }
        }

        @keyframes expand {
            0% { width: 50px; }
            50% { width: 100px; }
            100% { width: 50px; }
        }

        /* Header */
        .header {
            position: fixed;
            top: 0;
            width: 100%;
            z-index: 1000;
            background-color: rgba(0, 31, 63, 0.95);
            box-shadow: var(--shadow);
            transition: var(--transition);
            backdrop-filter: blur(10px);
            border-bottom: 1px solid rgba(230, 184, 0, 0.1);
        }

        .header.scrolled {
            background-color: rgba(0, 26, 53, 0.98);
            padding: 0.5rem 0;
        }

        .header.scrolled .logo-img {
            height: 50px;
        }

        .header-gradient {
            position: fixed;
            top: 80px;
            width: 100%;
            height: 30px;
            background: linear-gradient(to bottom, rgba(0,31,63,0.8), rgba(0,31,63,0));
            z-index: 999;
            pointer-events: none;
            transition: var(--transition);
        }

        .nav-container {
            max-width: 1440px;
            margin: 0 auto;
            padding: 1rem 2rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            transition: var(--transition);
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 1rem;
            cursor: pointer;
            text-decoration: none;
            position: relative;
        }

        .logo-img {
            height: 60px;
            border-radius: 50%;
            transition: var(--transition);
            background: transparent;
            border: 2px solid var(--gold);
            box-shadow: 0 0 10px rgba(230, 184, 0, 0.3);
            object-fit: cover;
        }

        .logo-text {
            color: var(--white);
            font-size: 1.2rem;
            font-weight: 600;
            letter-spacing: 0.5px;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
            position: relative;
        }

        .logo-text::after {
            content: 'Vizienfor Generation';
            position: absolute;
            bottom: -15px;
            left: 0;
            font-size: 0.7rem;
            color: var(--gold);
            width: 100%;
            text-align: center;
            font-weight: 400;
        }

        .logo:hover .logo-img {
            transform: scale(1.05);
            box-shadow: 0 0 15px rgba(230, 184, 0, 0.5);
            animation: pulse-gold 1.5s infinite;
        }

        @keyframes pulse-gold {
            0%, 100% { box-shadow: 0 0 15px rgba(230, 184, 0, 0.5); }
            50% { box-shadow: 0 0 20px rgba(230, 184, 0, 0.8); }
        }

        .nav-links {
            display: flex;
            gap: 2rem;
        }

        .nav-link {
            color: var(--white);
            text-decoration: none;
            font-weight: 500;
            position: relative;
            padding: 0.5rem 0;
            transition: var(--transition);
            font-size: 1rem;
            letter-spacing: 0.5px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .nav-link i {
            font-size: 0.8rem;
            opacity: 0;
            transition: var(--transition);
            transform: translateY(-5px);
        }

        .nav-link:hover i {
            opacity: 1;
            transform: translateY(0);
        }

        .nav-link::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            width: 0;
            height: 2px;
            background: var(--gold);
            transition: var(--transition);
        }

        .nav-link:hover::after {
            width: 100%;
        }

        .nav-link:hover {
            color: var(--gold-light);
        }

        .mobile-menu-btn {
            display: none;
            background: none;
            border: none;
            color: var(--white);
            font-size: 1.5rem;
            cursor: pointer;
            transition: var(--transition);
        }

        .mobile-menu-btn:hover {
            color: var(--gold);
            transform: rotate(90deg);
        }

        /* Hero Section */
        .hero {
            height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            background: var(--gradient);
            color: var(--white);
            position: relative;
            overflow: hidden;
            margin-top: 80px;
            text-align: center;
            scroll-snap-align: start;
        }

        .hero::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: url('https://images.unsplash.com/photo-1523050854058-8df90110c9f1?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80') no-repeat center center/cover;
            opacity: 0.15;
            z-index: 0;
        }

        .hero-content {
            position: relative;
            z-index: 1;
            padding: 2rem;
            max-width: 800px;
        }

        .hero-title {
            font-size: 3.5rem;
            margin-bottom: 1.5rem;
            line-height: 1.2;
            font-weight: 700;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
            animation: fadeInUp 1s ease-out;
            background: linear-gradient(to right, var(--gold), var(--gold-light));
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            position: relative;
            display: inline-block;
        }

        .hero-title::after {
            content: '';
            position: absolute;
            bottom: -10px;
            left: 50%;
            transform: translateX(-50%);
            width: 100px;
            height: 4px;
            background: var(--gold);
            border-radius: 2px;
        }

        .hero-text {
            font-size: 1.3rem;
            margin-bottom: 2.5rem;
            opacity: 0.9;
            animation: fadeInUp 1.2s ease-out;
            text-shadow: 0 1px 2px rgba(0, 0, 0, 0.2);
        }

        .hero-btn {
            display: inline-block;
            background: var(--gold);
            color: var(--navy);
            padding: 1rem 2.5rem;
            border-radius: 50px;
            text-decoration: none;
            font-weight: 600;
            transition: var(--transition);
            box-shadow: 0 4px 15px rgba(230, 184, 0, 0.3);
            animation: fadeInUp 1.4s ease-out;
            position: relative;
            overflow: hidden;
            border: none;
            cursor: pointer;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .hero-btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
            transition: 0.5s;
        }

        .hero-btn:hover::before {
            left: 100%;
        }

        .hero-btn:hover {
            background: var(--gold-light);
            transform: translateY(-3px);
            box-shadow: 0 6px 20px rgba(230, 184, 0, 0.4);
        }

        .hero-scroll {
            position: absolute;
            bottom: 2rem;
            left: 50%;
            transform: translateX(-50%);
            color: var(--white);
            font-size: 1.5rem;
            animation: bounce 2s infinite;
            cursor: pointer;
            background: rgba(255, 255, 255, 0.1);
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            backdrop-filter: blur(5px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            transition: var(--transition);
        }

        .hero-scroll:hover {
            background: rgba(230, 184, 0, 0.3);
            color: var(--gold-light);
        }

        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% { transform: translateY(0) translateX(-50%); }
            40% { transform: translateY(-20px) translateX(-50%); }
            60% { transform: translateY(-10px) translateX(-50%); }
        }

        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* Sections */
        .section {
            padding: 6rem 2rem;
            position: relative;
            scroll-snap-align: start;
        }

        .section-dark {
            background: var(--gray-light);
        }

        .section-title {
            text-align: center;
            font-size: 2.5rem;
            margin-bottom: 3rem;
            position: relative;
            color: var(--navy);
        }

        .section-title::after {
            content: '';
            position: absolute;
            bottom: -1rem;
            left: 50%;
            transform: translateX(-50%);
            width: 80px;
            height: 4px;
            background: var(--gold);
            border-radius: 2px;
        }

        .subsection-title {
            text-align: center;
            margin: 2rem 0;
            font-size: 2rem;
            color: var(--navy);
            position: relative;
            padding-bottom: 1rem;
        }

        .subsection-title::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 60px;
            height: 3px;
            background: var(--gold);
        }

        /* About Section */
        .about-content {
            max-width: 1200px;
            margin: 0 auto;
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 3rem;
            align-items: center;
        }

        .about-text {
            padding: 1.5rem;
        }

        .about-text h3 {
            font-size: 1.5rem;
            color: var(--navy);
            margin: 1.5rem 0 1rem;
            position: relative;
            padding-left: 1rem;
        }

        .about-text h3::before {
            content: '';
            position: absolute;
            left: 0;
            top: 0;
            height: 100%;
            width: 4px;
            background: var(--gold);
            border-radius: 2px;
        }

        .about-text ul {
            padding-left: 1.5rem;
            margin: 1rem 0;
        }

        .about-text ul li {
            margin-bottom: 0.5rem;
            position: relative;
            padding-left: 1.5rem;
        }

        .about-text ul li::before {
            content: '';
            position: absolute;
            left: 0;
            top: 0.7rem;
            width: 8px;
            height: 8px;
            background: var(--gold);
            border-radius: 50%;
        }

        .about-image-container {
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
            align-items: center;
            position: relative;
        }

        .about-image {
            width: 100%;
            max-height: 350px;
            object-fit: cover;
            border-radius: 1rem;
            box-shadow: var(--shadow);
            transition: var(--transition);
            position: relative;
            z-index: 1;
            border: 5px solid var(--white);
        }

        .about-image:nth-child(1) {
            transform: rotate(-3deg);
        }

        .about-image:nth-child(2) {
            transform: rotate(3deg);
        }

        .about-image:hover {
            transform: rotate(0) scale(1.03);
            z-index: 2;
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2);
        }

        .vision-text {
            text-align: center;
            margin-top: 2rem;
            padding: 1.5rem;
            background: rgba(230, 184, 0, 0.1);
            border-radius: 1rem;
            border: 1px solid rgba(230, 184, 0, 0.2);
            position: relative;
            overflow: hidden;
            backdrop-filter: blur(5px);
        }

        .vision-text::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: url('https://www.transparenttextures.com/patterns/diagmonds-light.png');
            opacity: 0.1;
            z-index: 0;
        }

        .vision-title {
            font-size: 2rem;
            color: var(--navy);
            margin-bottom: 0.5rem;
            position: relative;
            z-index: 1;
        }

        .vision-title::after {
            content: '';
            position: absolute;
            bottom: -10px;
            left: 50%;
            transform: translateX(-50%);
            width: 60px;
            height: 3px;
            background: var(--gold);
        }

        .vision-subtitle {
            font-size: 1.2rem;
            color: var(--navy-light);
            font-style: italic;
            margin-top: 1rem;
            position: relative;
            z-index: 1;
        }

        /* Stats Section */
        .stats-section {
            background: var(--gradient);
            color: var(--white);
            padding: 4rem 0;
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        .stats-section::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: url('https://www.transparenttextures.com/patterns/diagmonds.png');
            opacity: 0.1;
            z-index: 0;
        }

        .stats-container {
            max-width: 1200px;
            margin: 0 auto;
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 2rem;
            padding: 0 2rem;
            position: relative;
            z-index: 1;
        }

        .stat-item {
            padding: 2rem;
            position: relative;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 1rem;
            backdrop-filter: blur(5px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            transition: var(--transition);
        }

        .stat-item:hover {
            transform: translateY(-5px);
            background: rgba(255, 255, 255, 0.1);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
        }

        .stat-number {
            font-size: 3rem;
            font-weight: 700;
            margin-bottom: 0.5rem;
            color: var(--gold-light);
        }

        .stat-label {
            font-size: 1.1rem;
            opacity: 0.9;
        }

        .stat-item::after {
            content: '';
            position: absolute;
            right: 0;
            top: 50%;
            transform: translateY(-50%);
            height: 60%;
            width: 1px;
            background: rgba(255, 255, 255, 0.2);
        }

        .stat-item:last-child::after {
            display: none;
        }

        /* Kabinet Grid */
        .kabinet-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
            gap: 1.5rem;
            max-width: 1200px;
            margin: 0 auto;
        }

        .horizontal-grid {
            grid-template-columns: repeat(4, 1fr);
            max-width: 1000px;
            margin: 0 auto;
        }

        .member-card {
            background: var(--white);
            border-radius: 1rem;
            overflow: hidden;
            box-shadow: var(--shadow);
            transition: var(--transition);
            position: relative;
            text-align: center;
            border: 1px solid rgba(0, 0, 0, 0.1);
            cursor: pointer;
        }

        .member-card:hover {
            transform: translateY(-10px);
            box-shadow: var(--shadow-hover);
        }

        .member-image-container {
            position: relative;
            overflow: hidden;
            height: 350px;
        }

        .member-image {
            width: 100%;
            height: 100%;
            object-fit: cover;
            object-position: top center;
            transition: var(--transition);
        }

        .member-card:hover .member-image {
            transform: scale(1.05);
            opacity: 0.9;
        }

        .member-overlay {
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(to top, rgba(0, 31, 63, 0.8), transparent);
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            padding: 1.5rem;
            opacity: 0;
            transition: var(--transition);
        }

        .member-card:hover .member-overlay {
            opacity: 1;
        }

        .member-info {
            padding: 1.5rem;
            transition: var(--transition);
            background: var(--white);
        }

        .member-card:hover .member-info {
            transform: translateY(-50px);
        }

        .member-info h3 {
            font-size: 1.3rem;
            margin-bottom: 0.5rem;
            color: var(--navy);
            position: relative;
            display: inline-block;
        }

        .member-info h3::after {
            content: '';
            position: absolute;
            bottom: -5px;
            left: 50%;
            transform: translateX(-50%);
            width: 30px;
            height: 2px;
            background: var(--gold);
            transition: var(--transition);
        }

        .member-card:hover .member-info h3::after {
            width: 50px;
        }

        .member-info p {
            font-size: 0.9rem;
            color: var(--navy-light);
            font-weight: 500;
        }

        .member-social {
            display: flex;
            justify-content: center;
            gap: 1rem;
            margin-top: 1rem;
        }

        .member-social a {
            color: var(--white);
            background: rgba(255, 255, 255, 0.2);
            width: 35px;
            height: 35px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: var(--transition);
        }

        .member-social a:hover {
            background: var(--gold);
            color: var(--navy);
            transform: translateY(-3px);
        }

        /* Gallery */
        .gallery-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 1.5rem;
            max-width: 1200px;
            margin: 0 auto;
        }

        .gallery-item {
            border-radius: 1rem;
            overflow: hidden;
            transition: var(--transition);
            position: relative;
            cursor: pointer;
            box-shadow: var(--shadow);
            aspect-ratio: 4/3;
            border: 3px solid var(--white);
        }

        .gallery-item img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: var(--transition);
        }

        .gallery-item:hover {
            transform: translateY(-5px);
            box-shadow: var(--shadow-hover);
        }

        .gallery-item:hover img {
            transform: scale(1.05);
        }

        .gallery-caption {
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            padding: 1rem;
            background: linear-gradient(to top, rgba(0, 0, 0, 0.8), transparent);
            color: var(--white);
            opacity: 0;
            transition: var(--transition);
            transform: translateY(10px);
        }

        .gallery-item:hover .gallery-caption {
            opacity: 1;
            transform: translateY(0);
        }

        /* Testimonials */
        .testimonials-section {
            background: var(--gray-light);
            padding: 6rem 2rem;
            position: relative;
            overflow: hidden;
        }

        .testimonials-section::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: url('https://www.transparenttextures.com/patterns/light-wool.png');
            opacity: 0.1;
            z-index: 0;
        }

        .testimonials-container {
            max-width: 1200px;
            margin: 0 auto;
            position: relative;
            z-index: 1;
        }

        .testimonials-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
            gap: 2rem;
        }

        .testimonial-card {
            background: var(--white);
            padding: 2rem;
            border-radius: 1rem;
            box-shadow: var(--shadow);
            position: relative;
            transition: var(--transition);
            border: 1px solid rgba(0, 0, 0, 0.05);
        }

        .testimonial-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.1);
        }

        .testimonial-card::before {
            content: '"';
            position: absolute;
            top: 1rem;
            left: 1rem;
            font-size: 4rem;
            color: rgba(230, 184, 0, 0.1);
            font-family: serif;
            line-height: 1;
            z-index: 0;
        }

        .testimonial-content {
            margin-bottom: 1.5rem;
            font-style: italic;
            color: var(--navy-light);
            position: relative;
            z-index: 1;
        }

        .testimonial-author {
            display: flex;
            align-items: center;
            gap: 1rem;
        }

        .author-avatar {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            object-fit: cover;
            border: 2px solid var(--gold);
            transition: var(--transition);
        }

        .testimonial-card:hover .author-avatar {
            transform: scale(1.1);
            box-shadow: 0 0 0 3px rgba(230, 184, 0, 0.3);
        }

        .author-info h4 {
            color: var(--navy);
            margin-bottom: 0.2rem;
        }

        .author-info p {
            color: var(--navy-light);
            font-size: 0.9rem;
        }

        /* Timeline */
        .timeline-section {
            position: relative;
            padding: 6rem 0;
            background: var(--gray-light);
        }

        .timeline-container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 2rem;
            position: relative;
        }

        .timeline-container::before {
            content: '';
            position: absolute;
            top: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 2px;
            height: 100%;
            background: var(--gold);
        }

        .timeline-item {
            padding: 1.5rem;
            position: relative;
            width: 50%;
            margin-bottom: 2rem;
        }

        .timeline-item:nth-child(odd) {
            left: 0;
        }

        .timeline-item:nth-child(even) {
            left: 50%;
        }

        .timeline-item::after {
            content: '';
            position: absolute;
            top: 2rem;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: var(--gold);
            border: 4px solid var(--white);
            box-shadow: 0 0 0 4px var(--gold);
        }

        .timeline-item:nth-child(odd)::after {
            right: -10px;
        }

        .timeline-item:nth-child(even)::after {
            left: -10px;
        }

        .timeline-date {
            color: var(--gold-dark);
            font-weight: 600;
            margin-bottom: 0.5rem;
            display: flex;
            align-items: center;
        }

        .timeline-date i {
            margin-right: 0.5rem;
            color: var(--gold);
        }

        .timeline-content {
            background: var(--white);
            padding: 1.5rem;
            border-radius: 0.5rem;
            box-shadow: var(--shadow);
            transition: var(--transition);
            border-left: 4px solid var(--gold);
        }

        .timeline-item:hover .timeline-content {
            transform: translateY(-5px);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
        }

        .timeline-content h3 {
            color: var(--navy);
            margin-bottom: 0.5rem;
            position: relative;
            display: inline-block;
        }

        .timeline-content h3::after {
            content: '';
            position: absolute;
            bottom: -5px;
            left: 0;
            width: 30px;
            height: 2px;
            background: var(--gold);
            transition: var(--transition);
        }

        .timeline-item:hover .timeline-content h3::after {
            width: 50px;
        }

        /* Contact Section */
        .contact-container {
            max-width: 800px;
            margin: 0 auto;
            background: var(--white);
            border-radius: 1rem;
            padding: 3rem;
            box-shadow: var(--shadow);
            position: relative;
            overflow: hidden;
            border: 1px solid rgba(0, 0, 0, 0.05);
        }

        .contact-container::before {
            content: '';
            position: absolute;
            top: -50%;
            right: -50%;
            width: 100%;
            height: 200%;
            background: url('https://www.transparenttextures.com/patterns/diagmonds-light.png');
            opacity: 0.05;
            z-index: 0;
        }

        .contact-form {
            display: grid;
            gap: 1.5rem;
            position: relative;
            z-index: 1;
        }

        .form-group {
            position: relative;
        }

        .form-input {
            width: 100%;
            padding: 1.2rem 1.5rem;
            border: 2px solid var(--gray);
            border-radius: 0.8rem;
            transition: var(--transition);
            background: transparent;
            font-size: 1rem;
            color: var(--navy);
        }

        .form-input:focus {
            border-color: var(--gold);
            box-shadow: 0 0 0 3px rgba(230, 184, 0, 0.2);
            outline: none;
        }

        .form-label {
            position: absolute;
            left: 1rem;
            top: 1rem;
            padding: 0 0.5rem;
            background: var(--white);
            color: var(--navy-light);
            transition: var(--transition);
            pointer-events: none;
        }

        .form-input:focus + .form-label,
        .form-input:not(:placeholder-shown) + .form-label {
            top: -0.6rem;
            left: 1rem;
            font-size: 0.8rem;
            color: var(--gold);
            background: var(--white);
        }

        .submit-btn {
            background: var(--gold);
            color: var(--navy);
            padding: 1.2rem 2.5rem;
            border: none;
            border-radius: 0.8rem;
            cursor: pointer;
            font-weight: 600;
            transition: var(--transition);
            font-size: 1rem;
            margin-top: 1rem;
            box-shadow: 0 4px 15px rgba(230, 184, 0, 0.3);
            position: relative;
            overflow: hidden;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .submit-btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.4), transparent);
            transition: 0.5s;
        }

        .submit-btn:hover::before {
            left: 100%;
        }

        .submit-btn:hover {
            background: var(--gold-light);
            transform: translateY(-3px);
            box-shadow: 0 6px 20px rgba(230, 184, 0, 0.4);
        }

        /* Footer */
        .footer {
            background: var(--navy);
            color: var(--white);
            padding: 4rem 2rem 2rem;
            position: relative;
        }

        .footer::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 5px;
            background: linear-gradient(to right, var(--gold), var(--gold-light));
        }

        .footer-content {
            max-width: 1200px;
            margin: 0 auto;
            display: grid;
            grid-template-columns: 1.5fr 1fr;
            gap: 3rem;
        }

        .footer-info h3 {
            font-size: 1.5rem;
            margin-bottom: 1.5rem;
            color: var(--gold);
            position: relative;
            display: inline-block;
        }

        .footer-info h3::after {
            content: '';
            position: absolute;
            bottom: -5px;
            left: 0;
            width: 50%;
            height: 2px;
            background: var(--gold);
        }

        .footer-info p {
            margin-bottom: 1rem;
            opacity: 0.9;
            line-height: 1.6;
        }

        .footer-address {
            margin-top: 1.5rem;
        }

        .footer-address p {
            margin-bottom: 0.5rem;
            display: flex;
            align-items: flex-start;
            gap: 0.5rem;
        }

        .footer-address i {
            color: var(--gold);
            margin-top: 3px;
        }

        .footer-social h3 {
            font-size: 1.5rem;
            margin-bottom: 1.5rem;
            color: var(--gold);
            position: relative;
            display: inline-block;
        }

        .footer-social h3::after {
            content: '';
            position: absolute;
            bottom: -5px;
            right: 0;
            width: 50%;
            height: 2px;
            background: var(--gold);
        }

        .social-links {
            display: flex;
            justify-content: flex-end;
            gap: 1.5rem;
            flex-wrap: wrap;
        }

        .social-link {
            color: var(--white);
            font-size: 1.2rem;
            transition: var(--transition);
            width: 45px;
            height: 45px;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.1);
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .social-link:hover {
            color: var(--gold);
            background: rgba(255, 255, 255, 0.2);
            transform: translateY(-3px);
        }

        .footer-bottom {
            text-align: center;
            padding-top: 3rem;
            margin-top: 3rem;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            max-width: 1200px;
            margin: 3rem auto 0;
        }

        .footer-links {
            display: flex;
            justify-content: center;
            gap: 1.5rem;
            margin-bottom: 1.5rem;
        }

        .footer-link {
            color: var(--white);
            text-decoration: none;
            transition: var(--transition);
            position: relative;
        }

        .footer-link::after {
            content: '';
            position: absolute;
            bottom: -2px;
            left: 0;
            width: 0;
            height: 1px;
            background: var(--gold);
            transition: var(--transition);
        }

        .footer-link:hover {
            color: var(--gold);
        }

        .footer-link:hover::after {
            width: 100%;
        }

        .copyright {
            opacity: 0.8;
            font-size: 0.9rem;
        }

        /* Back to Top Button */
        .back-to-top {
            position: fixed;
            bottom: 2rem;
            right: 2rem;
            width: 50px;
            height: 50px;
            background: var(--gold);
            color: var(--navy);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.2rem;
            cursor: pointer;
            transition: var(--transition);
            opacity: 0;
            visibility: hidden;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
            z-index: 999;
            border: none;
        }

        .back-to-top.active {
            opacity: 1;
            visibility: visible;
        }

        .back-to-top:hover {
            background: var(--gold-light);
            transform: translateY(-3px);
            box-shadow: 0 4px 15px rgba(230, 184, 0, 0.4);
        }

        /* Notification */
        .notification {
            position: fixed;
            bottom: -100px;
            left: 50%;
            transform: translateX(-50%);
            background: var(--gold);
            color: var(--navy);
            padding: 1rem 2rem;
            border-radius: 50px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            z-index: 1000;
            transition: all 0.5s cubic-bezier(0.68, -0.55, 0.265, 1.55);
            font-weight: 500;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .notification.show {
            bottom: 20px;
        }

        .notification.success {
            background: #4CAF50;
            color: white;
        }

        .notification.error {
            background: #F44336;
            color: white;
        }

        .error-message {
            display: block;
            color: #F44336;
            font-size: 0.8rem;
            margin-top: 0.5rem;
            animation: fadeIn 0.3s ease-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Responsive Design */
        @media (max-width: 1024px) {
            .hero-title {
                font-size: 3rem;
            }
            
            .section-title {
                font-size: 2.2rem;
            }

            .about-content {
                grid-template-columns: 1fr;
            }

            .stats-container {
                grid-template-columns: repeat(2, 1fr);
            }

            .stat-item:nth-child(2)::after {
                display: none;
            }

            .timeline-container::before {
                left: 2rem;
            }

            .timeline-item {
                width: 100%;
                left: 0 !important;
                padding-left: 4rem;
            }

            .timeline-item::after {
                left: 1rem !important;
            }
            
            .horizontal-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }

        @media (max-width: 768px) {
            .nav-links {
                position: fixed;
                top: 80px;
                left: -100%;
                width: 100%;
                height: calc(100vh - 80px);
                background: var(--navy-dark);
                flex-direction: column;
                align-items: center;
                justify-content: center;
                gap: 2rem;
                transition: var(--transition);
                z-index: 998;
            }

            .nav-links.active {
                left: 0;
            }

            .mobile-menu-btn {
                display: block;
            }
            
            .hero-title {
                font-size: 2.5rem;
            }
            
            .section {
                padding: 4rem 1rem;
            }

            .footer-content {
                grid-template-columns: 1fr;
            }

            .footer-info, .footer-social {
                text-align: center;
            }

            .footer-info h3::after, .footer-social h3::after {
                left: 50%;
                transform: translateX(-50%);
                width: 100px;
            }

            .social-links {
                justify-content: center;
            }

            .stats-container {
                grid-template-columns: 1fr;
            }

            .stat-item::after {
                display: none;
            }
            
            .horizontal-grid {
                grid-template-columns: 1fr;
            }
        }

        @media (max-width: 480px) {
            .hero-title {
                font-size: 2rem;
            }

            .hero-text {
                font-size: 1.1rem;
            }

            .section-title {
                font-size: 1.8rem;
            }

            .kabinet-grid {
                grid-template-columns: 1fr;
            }

            .testimonials-grid {
                grid-template-columns: 1fr;
            }

            .contact-container {
                padding: 2rem 1.5rem;
            }
            
            .member-image-container {
                height: 400px;
            }
        }
    </style>
</head>
<body>
    <!-- Floating Particles Background -->
    <div class="particles" id="particles-js"></div>

    <!-- Preloader -->
    <div class="preloader">
        <div class="loader"></div>
        <div class="preloader-text">OSIS Vizienfor Generation</div>
    </div>

    <!-- Header -->
    <header class="header">
        <nav class="nav-container">
            <a href="#beranda" class="logo">
                <img src="Logo epi.jpeg" alt="OSIS Logo" class="logo-img">
                <img src="Logo OSIS SMA.jpg" alt="OSIS Logo" class="logo-img">
                <span class="logo-text">OSIS SMA FIBBIS 2024-2025</span>
            </a>
            <div class="nav-links">
                <a href="#beranda" class="nav-link">Beranda <i class="fas fa-home"></i></a>
                <a href="#tentang" class="nav-link">Tentang <i class="fas fa-info-circle"></i></a>
                <a href="#kabinet" class="nav-link">Kabinet <i class="fas fa-users"></i></a>
                <a href="#galeri" class="nav-link">Galeri <i class="fas fa-images"></i></a>
                <a href="#kontak" class="nav-link">Kontak <i class="fas fa-envelope"></i></a>
            </div>
            <button class="mobile-menu-btn">
                <i class="fas fa-bars"></i>
            </button>
        </nav>
    </header>

    <!-- Header Gradient -->
    <div class="header-gradient"></div>

    <!-- Hero Section -->
    <section class="hero" id="beranda">
        <div class="hero-content" data-aos="fade-up">
            <h1 class="hero-title">Membangun Pemimpin Masa Depan</h1>
            <p class="hero-text">Organisasi Siswa Intra Sekolah SMA FIBBIS Masa Bakti 2024-2025 dengan semangat "Vizienfor Generation"</p>
            <a href="#tentang" class="hero-btn">Jelajahi Lebih Lanjut</a>
        </div>
        <a href="#tentang" class="hero-scroll">
            <i class="fas fa-chevron-down"></i>
        </a>
    </section>

    <!-- Stats Section -->
    <section class="stats-section">
        <div class="stats-container">
            <div class="stat-item" data-aos="fade-up">
                <div class="stat-number" data-count="58">0</div>
                <div class="stat-label">Anggota Aktif</div>
            </div>
            <div class="stat-item" data-aos="fade-up" data-aos-delay="100">
                <div class="stat-number" data-count="40">0</div>
                <div class="stat-label">Program Kerja</div>
            </div>
            <div class="stat-item" data-aos="fade-up" data-aos-delay="200">
                <div class="stat-number" data-count="7">0</div>
                <div class="stat-label">Divisi</div>
            </div>
            <div class="stat-item" data-aos="fade-up" data-aos-delay="300">
                <div class="stat-number" data-count="100">0</div>
                <div class="stat-label">Dedikasi</div>
            </div>
        </div>
    </section>

    <!-- Tentang Section -->
    <section class="section" id="tentang">
        <h2 class="section-title" data-aos="fade-up">Tentang Kami</h2>
        <div class="about-content" data-aos="fade-up">
            <div class="about-text">
                <p>Organisasi Siswa Intra Sekolah (OSIS) adalah organisasi resmi yang dibentuk di lingkungan sekolah sebagai wadah bagi siswa untuk mengembangkan berbagai keterampilan, seperti kepemimpinan, kerja sama, kreativitas, serta tanggung jawab sosial. OSIS berfungsi sebagai sarana pembelajaran bagi siswa dalam berorganisasi, berkomunikasi, dan berkontribusi terhadap lingkungan sekolah serta masyarakat.
Sebagai organisasi yang berperan dalam dinamika kehidupan sekolah, OSIS berada di bawah bimbingan dan pengawasan langsung dari pihak sekolah, termasuk guru pembina dan kepala sekolah. Dengan adanya OSIS, siswa memiliki kesempatan untuk mengasah kemampuan dalam mengelola program dan kegiatan yang berorientasi pada pengembangan akademik, non-akademik, serta pembentukan karakter.</p>
                <h3>Visi Kami</h3>
                <p>Menjadi organisasi siswa terdepan dalam menciptakan generasi pemimpin yang berintegritas, kreatif, inovatif, dan berdaya saing global dengan semangat "Flowing Infinity and Beyond".</p>
                <h3>Misi Kami</h3>
                <ul>
                    <li>Mengembangkan program yang mendukung pengembangan karakter siswa berbasis nilai-nilai keislaman</li>
                    <li>Menciptakan lingkungan belajar yang inklusif, kolaboratif, dan berwawasan global</li>
                    <li>Mendorong inovasi dan kreativitas siswa melalui berbagai kegiatan positif</li>
                    <li>Membangun jejaring dengan berbagai pihak untuk pengembangan potensi siswa</li>
                    <li>Menjadi penghubung antara siswa dengan pihak sekolah dan masyarakat</li>
                </ul>
            </div>
            <div class="about-image-container">
                <img src="IMG_0700.JPG" alt="Kegiatan OSIS" class="about-image" data-aos="fade-up">
                <img src="IMG_0711.JPG" alt="Team OSIS" class="about-image" data-aos="fade-up" data-aos-delay="100">
                <div class="vision-text" data-aos="fade-up">
                    <h3 class="vision-title">Vizienfor Generation</h3>
                    <p class="vision-subtitle">"Flowing Infinity and Beyond"</p>
                </div>
            </div>
        </div>
    </section>

    <!-- Timeline Section -->
    <section class="section section-dark">
        <h2 class="section-title" data-aos="fade-up">Timeline Kegiatan</h2>
        <div class="timeline-container" data-aos="fade-up">
            <div class="timeline-item">
                <div class="timeline-date"><i class="fas fa-calendar-alt"></i> November 2024</div>
                <div class="timeline-content">
                    <h3>Pelaksanaan Latihan Dasar Kepemimpinan Organisasi</h3>
                    <p>Acara Persiapan dan Pendidikan Baik secara Fisik dan Mental Bagi Calon Pengurus OSIS. </p>
                </div>
            </div>
            <div class="timeline-item">
                <div class="timeline-date"><i class="fas fa-calendar-alt"></i> Desember 2024</div>
                <div class="timeline-content">
                    <h3>Serah Terima Jabatan Dari Pengurus OSIS Sebelumnya</h3>
                    <p>Pelaksaan Serah Terima Jabatan (Sertijab) Kepengurusan OSIS dari Pengurus Periode Sebelumbya.</p>
                </div>
            </div>
            <div class="timeline-item">
                <div class="timeline-date"><i class="fas fa-calendar-alt"></i> Desember 2024</div>
                <div class="timeline-content">
                    <h3>Pekan Prestasi 2024</h3>
                    <p>Adalah Acara yang Diadakan Untuk Mengasah Skill dan Juga Mempererat Hubungan Silaturahmi Antar Siswa, Acara Ini Juga Menjadi Acara Pertama Bagi Kepengurusan OSIS Kami.</p>
                </div>
            </div>
            <div class="timeline-item">
                <div class="timeline-date"><i class="fas fa-calendar-alt"></i> Februari 2025</div>
                <div class="timeline-content">
                    <h3>Islamic Scout Camp (ISC 2025)</h3>
                    <p>Adalah Kegiatan Mukhoyyam (Camping) yang diikuti Oleh Siswa Kelas 7 dan 8 SMP, dan 10 SMA, Acara ini bertempat di PPTK Gambung Pada Tanggal 24-26 Februari Dengan Tema "Menjelajah Alam Dengan Takwa, Memupuk Kemandirian dalam Bingkai Islami."</p>
                </div>
            </div>
            <div class="timeline-item">
                <div class="timeline-date"><i class="fas fa-calendar-alt"></i> Maret 2025</div>
                <div class="timeline-content">
                    <h3>Gebyar Ramadhan 2025</h3>
                    <p>Adalah Kegiatan dalam Rangka Menyambut Datangnya Bulan Suci Ramadhan yang Terdiri dari Karnaval, Tebar Ta'jil, Bazaar Pakaian Murah, Tasmi' Akbar, Sembako Murah, dan Acara Puncak Yaitu Lomba Antar DTA Se-Cimaung dan Tabligh Akbar yang Dilaksanakan Pada Tanggal 20 Maret 2025.</p>
                  </div>
            </div>
        </div>
    </section>

   <!-- Kabinet Section -->
<section class="section" id="kabinet">
    <h2 class="section-title" data-aos="fade-up">Struktur Kabinet</h2>
    
    <h3 class="subsection-title" data-aos="fade-up">OSIS Inti</h3>
    <div class="kabinet-grid horizontal-grid">
        <!-- Ketua OSIS -->
        <div class="member-card" data-aos="fade-up">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Ketua OSIS" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="https://www.instagram.com/osis_fibbis" class="social-link" target="_blank">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Muhammad Ezar Munggaran</h3>
                <p>Ketua OSIS</p>
            </div>
        </div>
        
        <!-- Wakil Ketua OSIS -->
        <div class="member-card" data-aos="fade-up" data-aos-delay="100">
            <div class="member-image-container">
                <img src="DSC_0074-removebg-preview.png" alt="Wakil Ketua OSIS" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Muhammad Alfafa Rizki Deshan</h3>
                <p>Wakil Ketua OSIS</p>
            </div>
        </div>

        <!-- Sekretaris OSIS -->
        <div class="member-card" data-aos="fade-up" data-aos-delay="200">
            <div class="member-image-container">
                <img src="DSC_0021-removebg-preview.png" alt="Sekretaris OSIS" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Muhammad Hasan Al-Banna S</h3>
                <p>Sekretaris OSIS</p>
            </div>
        </div>

        <!-- Bendahara OSIS -->
        <div class="member-card" data-aos="fade-up" data-aos-delay="300">
            <div class="member-image-container">
                <img src="DSC_0021-removebg-preview.png" alt="Bendahara OSIS" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Ramzy Hermansyah</h3>
                <p>Bendahara OSIS</p>
            </div>
        </div>
    </div>

    <!-- Additional 4 photos for OSIS Inti -->
    <div class="kabinet-grid horizontal-grid" style="margin-top: 2rem;">
        <div class="member-card" data-aos="fade-up">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Anggota OSIS Inti" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Nama Anggota 1</h3>
                <p>Jabatan Anggota</p>
            </div>
        </div>
        
        <div class="member-card" data-aos="fade-up" data-aos-delay="100">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Anggota OSIS Inti" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Nama Anggota 2</h3>
                <p>Jabatan Anggota</p>
            </div>
        </div>
        
        <div class="member-card" data-aos="fade-up" data-aos-delay="200">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Anggota OSIS Inti" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Nama Anggota 3</h3>
                <p>Jabatan Anggota</p>
            </div>
        </div>
        
        <div class="member-card" data-aos="fade-up" data-aos-delay="300">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Anggota OSIS Inti" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Nama Anggota 4</h3>
                <p>Jabatan Anggota</p>
            </div>
        </div>
    </div>

    <h3 class="subsection-title" data-aos="fade-up">OSIS Kedisiplinan</h3>
    <div class="kabinet-grid" style="justify-content: center;">
        <!-- Ketua OSIS Kedisiplinan -->
        <div class="member-card" data-aos="fade-up">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Ketua OSIS Kedisiplinan" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Rizky Ramdani Putra Mokoginta</h3>
                <p>Ketua OSIS Kedisiplinan</p>
            </div>
        </div>

        <!-- Anggota OSIS Kedisiplinan 1 -->
        <div class="member-card" data-aos="fade-up" data-aos-delay="100">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Anggota OSIS Kedisiplinan" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Muhammad Arfa Hegel Muttahhari</h3>
                <p>Anggota OSIS Kedisiplinan</p>
            </div>
        </div>

        <!-- Anggota OSIS Kedisiplinan 2 -->
        <div class="member-card" data-aos="fade-up" data-aos-delay="200">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Anggota OSIS Kedisiplinan" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Muhammad Ibrahim</h3>
                <p>Anggota OSIS Kedisiplinan</p>
            </div>
        </div>
    </div>

    <!-- Additional 4 photos for OSIS Kedisiplinan -->
    <div class="kabinet-grid horizontal-grid" style="margin-top: 2rem; justify-content: center;">
        <div class="member-card" data-aos="fade-up">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Anggota OSIS Kedisiplinan" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Nama Anggota 1</h3>
                <p>Anggota OSIS Kedisiplinan</p>
            </div>
        </div>
        
        <div class="member-card" data-aos="fade-up" data-aos-delay="100">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Anggota OSIS Kedisiplinan" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Nama Anggota 2</h3>
                <p>Anggota OSIS Kedisiplinan</p>
            </div>
        </div>
        
        <div class="member-card" data-aos="fade-up" data-aos-delay="200">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Anggota OSIS Kedisiplinan" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Nama Anggota 3</h3>
                <p>Anggota OSIS Kedisiplinan</p>
            </div>
        </div>
        
        <div class="member-card" data-aos="fade-up" data-aos-delay="300">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Anggota OSIS Kedisiplinan" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Nama Anggota 4</h3>
                <p>Anggota OSIS Kedisiplinan</p>
            </div>
        </div>
    </div>

    <h3 class="subsection-title" data-aos="fade-up">OSIS Kesenian</h3>
    <div class="kabinet-grid" style="justify-content: center;">
        <!-- Ketua OSIS Kesenian -->
        <div class="member-card" data-aos="fade-up">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Ketua OSIS Kesenian" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Nama Ketua</h3>
                <p>Ketua OSIS Kesenian</p>
            </div>
        </div>

        <!-- Anggota OSIS Kesenian 1 -->
        <div class="member-card" data-aos="fade-up" data-aos-delay="100">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Anggota OSIS Kesenian" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Nama Anggota 1</h3>
                <p>Anggota OSIS Kesenian</p>
            </div>
        </div>
    </div>

    <!-- Additional 4 photos for OSIS Kesenian -->
    <div class="kabinet-grid horizontal-grid" style="margin-top: 2rem; justify-content: center;">
        <div class="member-card" data-aos="fade-up">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Anggota OSIS Kesenian" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Nama Anggota 1</h3>
                <p>Anggota OSIS Kesenian</p>
            </div>
        </div>
        
        <div class="member-card" data-aos="fade-up" data-aos-delay="100">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Anggota OSIS Kesenian" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Nama Anggota 2</h3>
                <p>Anggota OSIS Kesenian</p>
            </div>
        </div>
        
        <div class="member-card" data-aos="fade-up" data-aos-delay="200">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Anggota OSIS Kesenian" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Nama Anggota 3</h3>
                <p>Anggota OSIS Kesenian</p>
            </div>
        </div>
        
        <div class="member-card" data-aos="fade-up" data-aos-delay="300">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Anggota OSIS Kesenian" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Nama Anggota 4</h3>
                <p>Anggota OSIS Kesenian</p>
            </div>
        </div>
    </div>

    <h3 class="subsection-title" data-aos="fade-up">OSIS Olahraga</h3>
    <div class="kabinet-grid" style="justify-content: center;">
        <!-- Ketua OSIS Olahraga -->
        <div class="member-card" data-aos="fade-up">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Ketua OSIS Olahraga" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Nama Ketua</h3>
                <p>Ketua OSIS Olahraga</p>
            </div>
        </div>

        <!-- Anggota OSIS Olahraga 1 -->
        <div class="member-card" data-aos="fade-up" data-aos-delay="100">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Anggota OSIS Olahraga" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Nama Anggota 1</h3>
                <p>Anggota OSIS Olahraga</p>
            </div>
        </div>
    </div>

    <!-- Additional 3 photos for OSIS Olahraga -->
    <div class="kabinet-grid horizontal-grid" style="margin-top: 2rem; justify-content: center;">
        <div class="member-card" data-aos="fade-up">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Anggota OSIS Olahraga" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Nama Anggota 1</h3>
                <p>Anggota OSIS Olahraga</p>
            </div>
        </div>
        
        <div class="member-card" data-aos="fade-up" data-aos-delay="100">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Anggota OSIS Olahraga" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Nama Anggota 2</h3>
                <p>Anggota OSIS Olahraga</p>
            </div>
        </div>
        
        <div class="member-card" data-aos="fade-up" data-aos-delay="200">
            <div class="member-image-container">
                <img src="DSC_0021.png" alt="Anggota OSIS Olahraga" class="member-image">
                <div class="member-overlay">
                    <div class="member-social">
                        <a href="#" class="social-link">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="social-link">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                    </div>
                </div>
            </div>
            <div class="member-info">
                <h3>Nama Anggota 3</h3>
                <p>Anggota OSIS Olahraga</p>
            </div>
        </div>
    </div>
</section>

    <!-- Testimonials Section -->
    <section class="testimonials-section">
        <h2 class="section-title" data-aos="fade-up">Apa Kata Mereka?</h2>
        <div class="testimonials-container">
            <div class="testimonials-grid">
                <div class="testimonial-card" data-aos="fade-up">
                    <div class="testimonial-content">
                        "Kata Kata."
                    </div>
                    <div class="testimonial-author">
                        <img src="IMG_0456.JPG" alt="Testimoni 1" class="author-avatar">
                        <div class="author-info">
                            <h4>Devi Nur Muhammad Siddiq, S.PdI.</h4>
                            <p>Kepala Sekolah SMA FIBBIS</p>
                        </div>
                    </div>
                </div>
                
                <div class="testimonial-card" data-aos="fade-up" data-aos-delay="100">
                    <div class="testimonial-content">
                        "madep."
                    </div>
                    <div class="testimonial-author">
                        <img src="IMG_0493.JPG" alt="Testimoni 2" class="author-avatar">
                        <div class="author-info">
                            <h4>Ridwan Gunawan, S.Pd.</h4>
                            <p>Pembina OSIS 2024-2025</p>
                        </div>
                    </div>
                </div>
                
                <div class="testimonial-card" data-aos="fade-up" data-aos-delay="200">
                    <div class="testimonial-content">
                        "anjay."
                    </div>
                    <div class="testimonial-author">
                        <img src="IMG_0456.JPG" alt="Testimoni 3" class="author-avatar">
                        <div class="author-info">
                            <h4>ppppp</h4>
                            <p>ppppp</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Galeri Section -->
    <section class="section" id="galeri">
        <h2 class="section-title" data-aos="fade-up">Galeri Kegiatan</h2>
        <div class="gallery-grid">
            <a href="IMG_0711.JPG" data-lightbox="gallery" class="gallery-item" data-aos="fade-up">
                <img src="IMG_0711.JPG" alt="Pekan Prestasi 2024">
                <div class="gallery-caption">Pekan Prestasi 2024</div>
            </a>
            <a href="IMG_0700.JPG" data-lightbox="gallery" class="gallery-item" data-aos="fade-up" data-aos-delay="100">
                <img src="IMG_0700.JPG" alt="Pelantikan OSIS">
                <div class="gallery-caption">Pelantikan OSIS</div>
            </a>
            <a href="https://images.unsplash.com/photo-1523050854058-8df90110c9f1?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80" data-lightbox="gallery" class="gallery-item" data-aos="fade-up" data-aos-delay="200">
                <img src="https://images.unsplash.com/photo-1523050854058-8df90110c9f1?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80" alt="Kegiatan Belajar">
                <div class="gallery-caption">Kegiatan Belajar</div>
            </a>
            <a href="https://images.unsplash.com/photo-1522202176988-66273c2fd55f?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80" data-lightbox="gallery" class="gallery-item" data-aos="fade-up" data-aos-delay="300">
                <img src="https://images.unsplash.com/photo-1522202176988-66273c2fd55f?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80" alt="Kerja Tim">
                <div class="gallery-caption">Kerja Tim OSIS</div>
            </a>
        </div>
    </section>

    <!-- Kontak Section -->
    <section class="section" id="kontak">
        <div class="contact-container" data-aos="fade-up">
            <h2 class="section-title">Hubungi Kami</h2>
            <form class="contact-form" id="contactForm" method="POST" action="process_form.php">
                <div class="form-group">
                    <input type="text" class="form-input" id="name" name="name" placeholder=" " required>
                    <label for="name" class="form-label">Nama Lengkap</label>
                </div>
                <div class="form-group">
                    <input type="email" class="form-input" id="email" name="email" placeholder=" " required>
                    <label for="email" class="form-label">Alamat Email</label>
                </div>
                <div class="form-group">
                    <input type="text" class="form-input" id="subject" name="subject" placeholder=" " required>
                    <label for="subject" class="form-label">Subjek</label>
                </div>
                <div class="form-group">
                    <textarea class="form-input" id="message" name="message" rows="5" placeholder=" " required></textarea>
                    <label for="message" class="form-label">Pesan Anda</label>
                </div>
                <button type="submit" class="submit-btn">Kirim Pesan</button>
            </form>
        </div>
    </section>

    <!-- Footer -->
    <footer class="footer">
        <div class="footer-content">
            <div class="footer-info">
                <h3>SMA Fithrah Insani Bilingual Boarding Islamic School</h3>
                <p>Organisasi Siswa Intra Sekolah (OSIS) masa bakti 2024/2025 dengan semangat "Vizienfor Generation" untuk membangun pemimpin masa depan.</p>
                
                <div class="footer-address">
                    <p><i class="fas fa-map-marker-alt"></i> Jl Cigondok Kp Cileumeuh, RT.02/RW.02,</p>
                    <p><i class="fas fa-map-pin"></i> Pasirhuni, Kec. Cimaung, Kabupaten Bandung, Jawa Barat 40374</p>
                    <p><i class="fas fa-envelope"></i> osisfibbis1@gmail.com</p>
                    <p><i class="fas fa-phone"></i> 085856330728</p>
                </div>
            </div>
            
            <div class="footer-social">
                <h3>Media Sosial</h3>
                <div class="social-links">
                    <a href="https://www.instagram.com/osis_fibbis" class="social-link" target="_blank">
                        <i class="fab fa-instagram"></i>
                    </a>
                    <a href="https://www.tiktok.com/@osis.fibbis" class="social-link" target="_blank">
                        <i class="fab fa-tiktok"></i>
                    </a>
                    <a href="https://www.youtube.com/@pesantrenfi" class="social-link" target="_blank">
                        <i class="fab fa-youtube"></i>
                    </a>
                    <a href="#" class="social-link">
                        <i class="fab fa-facebook-f"></i>
                    </a>
                    <a href="#" class="social-link">
                        <i class="fab fa-twitter"></i>
                    </a>
                    <a href="#" class="social-link">
                        <i class="fab fa-whatsapp"></i>
                    </a>
                </div>
            </div>
        </div>
        
        <div class="footer-bottom">
            <div class="footer-links">
                <a href="#beranda" class="footer-link">Beranda</a>
                <a href="#tentang" class="footer-link">Tentang</a>
                <a href="#kabinet" class="footer-link">Kabinet</a>
                <a href="#galeri" class="footer-link">Galeri</a>
                <a href="#kontak" class="footer-link">Kontak</a>
            </div>
            <p class="copyright">&copy; 2025 OSIS SMA FIBBIS. Hak Cipta Dilindungi. | Dibuat Oleh Pengurus OSIS Masa Bakti 2024-2025
    </footer>

    <!-- Back to Top Button -->
    <div class="back-to-top">
        <i class="fas fa-arrow-up"></i>
    </div>

    <!-- Notification -->
    <div class="notification" id="notification">
        <i class="fas fa-check-circle"></i>
        <span id="notificationText">Pesan berhasil dikirim!</span>
    </div>

    <script src="https://unpkg.com/aos@2.3.1/dist/aos.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/lightbox2/2.11.4/js/lightbox.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/particles.js@2.0.0/particles.min.js"></script>
    <script>
        // Preloader
        window.addEventListener('load', () => {
            setTimeout(() => {
                document.querySelector('.preloader').style.opacity = '0';
                setTimeout(() => {
                    document.querySelector('.preloader').style.display = 'none';
                }, 500);
            }, 1500);
        });

        // Initialize AOS
        AOS.init({
            duration: 800,
            once: true,
            easing: 'ease-in-out-quad'
        });

        // Mobile Menu Toggle
        const mobileMenuBtn = document.querySelector('.mobile-menu-btn');
        const navLinks = document.querySelector('.nav-links');

        mobileMenuBtn.addEventListener('click', () => {
            navLinks.classList.toggle('active');
            mobileMenuBtn.innerHTML = navLinks.classList.contains('active') ? 
                '<i class="fas fa-times"></i>' : '<i class="fas fa-bars"></i>';
        });

        // Close mobile menu when clicking a link
        document.querySelectorAll('.nav-link').forEach(link => {
            link.addEventListener('click', () => {
                if (navLinks.classList.contains('active')) {
                    navLinks.classList.remove('active');
                    mobileMenuBtn.innerHTML = '<i class="fas fa-bars"></i>';
                }
            });
        });

        // Smooth scroll for all anchor links
        document.querySelectorAll('a[href^="#"]').forEach(anchor => {
            anchor.addEventListener('click', function(e) {
                e.preventDefault();
                document.querySelector(this.getAttribute('href')).scrollIntoView({
                    behavior: 'smooth'
                });
            });
        });

        // Form submission
        document.getElementById('contactForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            // Simulate form submission
            setTimeout(() => {
                showNotification('Pesan Anda telah berhasil dikirim!');
                this.reset();
            }, 1000);
        });

        // Notification function
        function showNotification(message) {
            const notification = document.getElementById('notification');
            const notificationText = document.getElementById('notificationText');
            
            notificationText.textContent = message;
            notification.classList.add('show');
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }

        // Header scroll effect
        window.addEventListener('scroll', () => {
            const header = document.querySelector('.header');
            const headerGradient = document.querySelector('.header-gradient');
            const backToTop = document.querySelector('.back-to-top');
            
            if (window.scrollY > 50) {
                header.classList.add('scrolled');
                headerGradient.style.opacity = '0.8';
            } else {
                header.classList.remove('scrolled');
                headerGradient.style.opacity = '1';
            }

            // Back to top button
            if (window.scrollY > 300) {
                backToTop.classList.add('active');
            } else {
                backToTop.classList.remove('active');
            }
        });

        // Back to top button functionality
        document.querySelector('.back-to-top').addEventListener('click', () => {
            window.scrollTo({
                top: 0,
                behavior: 'smooth'
            });
        });

        // Lightbox initialization
        lightbox.option({
            'resizeDuration': 200,
            'wrapAround': true,
            'fadeDuration': 300,
            'albumLabel': 'Gambar %1 dari %2'
        });

        // Animated counter for stats
        const counters = document.querySelectorAll('.stat-number');
        const speed = 200;

        function animateCounters() {
            counters.forEach(counter => {
                const target = +counter.getAttribute('data-count');
                const count = +counter.innerText;
                const increment = target / speed;

                if (count < target) {
                    counter.innerText = Math.ceil(count + increment);
                    setTimeout(animateCounters, 1);
                } else {
                    counter.innerText = target;
                }
            });
        }

        // Start counter animation when stats section is in view
        const statsSection = document.querySelector('.stats-section');
        const observer = new IntersectionObserver((entries) => {
            if (entries[0].isIntersecting) {
                animateCounters();
                observer.unobserve(statsSection);
            }
        }, { threshold: 0.5 });

        observer.observe(statsSection);

        // Floating labels for form inputs
        document.querySelectorAll('.form-input').forEach(input => {
            input.addEventListener('focus', function() {
                this.nextElementSibling.style.color = 'var(--gold)';
            });
            
            input.addEventListener('blur', function() {
                if (!this.value) {
                    this.nextElementSibling.style.color = 'var(--navy-light)';
                }
            });
        });

        // Particles.js configuration
        particlesJS('particles-js', {
            "particles": {
                "number": {
                    "value": 80,
                    "density": {
                        "enable": true,
                        "value_area": 800
                    }
                },
                "color": {
                    "value": "#e6b800"
                },
                "shape": {
                    "type": "circle",
                    "stroke": {
                        "width": 0,
                        "color": "#000000"
                    },
                    "polygon": {
                        "nb_sides": 5
                    }
                },
                "opacity": {
                    "value": 0.5,
                    "random": false,
                    "anim": {
                        "enable": false,
                        "speed": 1,
                        "opacity_min": 0.1,
                        "sync": false
                    }
                },
                "size": {
                    "value": 3,
                    "random": true,
                    "anim": {
                        "enable": false,
                        "speed": 40,
                        "size_min": 0.1,
                        "sync": false
                    }
                },
                "line_linked": {
                    "enable": true,
                    "distance": 150,
                    "color": "#e6b800",
                    "opacity": 0.2,
                    "width": 1
                },
                "move": {
                    "enable": true,
                    "speed": 2,
                    "direction": "none",
                    "random": false,
                    "straight": false,
                    "out_mode": "out",
                    "bounce": false,
                    "attract": {
                        "enable": false,
                        "rotateX": 600,
                        "rotateY": 1200
                    }
                }
            },
            "interactivity": {
                "detect_on": "canvas",
                "events": {
                    "onhover": {
                        "enable": true,
                        "mode": "grab"
                    },
                    "onclick": {
                        "enable": true,
                        "mode": "push"
                    },
                    "resize": true
                },
                "modes": {
                    "grab": {
                        "distance": 140,
                        "line_linked": {
                            "opacity": 1
                        }
                    },
                    "bubble": {
                        "distance": 400,
                        "size": 40,
                        "duration": 2,
                        "opacity": 8,
                        "speed": 3
                    },
                    "repulse": {
                        "distance": 200,
                        "duration": 0.4
                    },
                    "push": {
                        "particles_nb": 4
                    },
                    "remove": {
                        "particles_nb": 2
                    }
                }
            },
            "retina_detect": true
        });

        // Parallax effect for hero section
        window.addEventListener('scroll', function() {
            const hero = document.querySelector('.hero');
            const scrollPosition = window.pageYOffset;
            hero.style.backgroundPositionY = scrollPosition * 0.5 + 'px';
        });

        // Dynamic year for copyright
        document.querySelector('.copyright').innerHTML += new Date().getFullYear();
    </script>
</body>
</html>
