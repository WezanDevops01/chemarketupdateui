2:40

second 5:17





<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Broadcast Requests - Quotation</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">

    <link href="https://cdn.datatables.net/1.13.8/css/dataTables.bootstrap5.min.css" rel="stylesheet">
    <link href="https://cdn.datatables.net/responsive/2.5.0/css/responsive.bootstrap5.min.css" rel="stylesheet">

    <link href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.1/font/bootstrap-icons.css" rel="stylesheet">

    <style>
        body {
            background-color: #f6f7f9;
            overflow-x: hidden;
        }

        /* Sidebar */
        .sidebar {
            width: 280px;
            height: 100vh;
            background: #0f172a;
            color: #fff;
            position: fixed;
            left: 0;
            top: 0;
            z-index: 1050;
            transition: transform 0.3s ease;
        }

        .sidebar a {
            color: #cbd5e1;
            text-decoration: none;
            padding: 10px 15px;
            display: block;
            border-radius: 6px;
            margin-bottom: 5px;
        }

        .sidebar a.active,
        .sidebar a:hover {
            background: #1e293b;
            color: #fff;
        }

        .sidebar .section-title {
            font-size: 12px;
            text-transform: uppercase;
            color: #94a3b8;
            margin: 20px 15px 10px;
        }

        /* Main Content */
        .main-content {
            margin-left: 280px;
            padding: 20px;
            transition: margin-left 0.3s ease;
        }

        /* Mobile Adjustments */
        @media (max-width: 991px) {
            .sidebar {
                transform: translateX(-100%);
            }
            .sidebar.show {
                transform: translateX(0);
            }
            .main-content {
                margin-left: 0;
            }
            .overlay {
                display: none;
                position: fixed;
                top: 0;
                left: 0;
                width: 100%;
                height: 100%;
                background: rgba(0,0,0,0.5);
                z-index: 1040;
            }
            .overlay.active {
                display: block;
            }
        }

        /* Header */
        .top-bar {
            padding: 15px 20px;
            border-radius: 10px;
        }

        /* Badges */
        .badge-active {
            background: #F3FFF7;
            color: #365A42;
            padding: 10px 20px;
            box-shadow: rgba(60, 64, 67, 0.3) 0px 1px 2px 0px, rgba(60, 64, 67, 0.15) 0px 1px 3px 1px;
            border-radius: 30px;
        }

        .badge-expired {
            background: #FFFBFB;
            color: #CF3D3D;
            padding: 10px 20px;
            box-shadow: rgba(60, 64, 67, 0.3) 0px 1px 2px 0px, rgba(60, 64, 67, 0.15) 0px 1px 3px 1px;
            border-radius: 30px;
        }

        .badge-broadcast {
            background: #365A42;
            color: #fff;
            border-radius: 10px;
            padding: 10px;
            font-size: 14px;
            font-weight: 500;
        }

        .badge-new {
            background: #dc3545;
            color: #fff;
        }

        /* Table card and Scroll */
        .card-custom {
            border-radius: 15px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.05);
            background: #fff;
            overflow: hidden;
        }

        .table-responsive-custom {
            overflow-x: auto;
            -webkit-overflow-scrolling: touch;
        }

        .btn-view {
            border-radius: 20px;
            padding: 5px 20px;
        }

        thead {
            background-color: #1E293B !important;
            color: white;
        }

        .add-new-request {
            background-color: #1E293B;
            color: #FFFFFF;
            border-radius: 30px;
        }

        .top-header {
            height: 64px;
            background: #fff;
        }
        
        .cursor-pointer {
            cursor: pointer;
        }
        
        .search-wrapper input {
            height: 42px;
            font-size: 14px;
        }

        .bg-danger {
            right: -28px;
            font-size: 11px;
        }

        .cart-icon {
            margin-right: 2rem;
        }

        .filters-wrapper {
            display: flex;
            gap: 16px;
            align-items: center;
            position: relative;
            justify-content: end;
            margin-bottom: 10px;
        }

        .filter-pill {
            display: flex;
            align-items: center;
            gap: 8px;
            padding: 12px 18px;
            border: 1px solid #e5e7eb;
            border-radius: 18px;
            background-color: #ffffff;
            font-size: 14px;
            color: #111827;
            cursor: pointer;
            user-select: none;
        }

        .filter-dropdown {
            position: absolute;
            top: calc(100% + 8px);
            left: 0;
            min-width: 160px;
            background: #ffffff;
            border: 1px solid #e5e7eb;
            border-radius: 12px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.08);
            display: none;
            z-index: 1000;
        }

        .user-bar {
            display: flex;
            align-items: center;
            justify-content: space-between;
            border-radius: 14px;
            padding: 14px 18px;
            color: #ffffff;
        }

        .avatar {
            width: 54px;
            height: 54px;
            border-radius: 50%;
            overflow: hidden;
            border: 2px solid rgba(255,255,255,0.3);
        }

        .avatar img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        /* Mobile specific hiding for search/labels */
        @media (max-width: 576px) {
            .search-wrapper { display: none; }
            .top-header .fw-semibold { font-size: 16px; }
            .filters-wrapper { justify-content: start; overflow-x: auto; padding-bottom: 10px; }
        }
    </style>
</head>
<body>

<div class="overlay" id="sidebarOverlay"></div>

<div class="sidebar p-3" id="mainSidebar">
    <div class="user-bar">
        <div class="user-info">
            <div class="avatar">
                <img src="https://i.pravatar.cc/100?img=32" alt="User">
            </div>
            <div class="user-text">
                <div class="user-name">
                    Watson An...
                    <i class="bi bi-caret-down-fill"></i>
                </div>
                <div class="user-email">
                    Watsonbuyer@gm...
                </div>
            </div>
        </div>
        <div class="user-action d-lg-none" id="closeSidebar">
            <i class="bi bi-x-lg"></i>
        </div>
    </div>

    <div class="section-title">Procurement</div>
    <a href="#" class="active"><i class="bi bi-megaphone"></i> Broadcast RFQ</a>
    <a href="#"><i class="bi bi-send"></i> Direct RFQ</a>
    <a href="#"><i class="bi bi-box"></i> Orders Management</a>
    <a href="#"><i class="bi bi-flask"></i> Samples Request</a>
    <a href="#"><i class="bi bi-check2-circle"></i> My Approved Products</a>

    <div class="section-title">Account</div>
    <a href="#"><i class="bi bi-person"></i> Profile</a>
    <a href="#"><i class="bi bi-geo"></i> Addresses</a>
    <a href="#"><i class="bi bi-people"></i> Manage Staff</a>

    <div class="section-title">Selling</div>
    <a href="#"><i class="bi bi-shop"></i> Become a Seller</a>
</div>

<div class="main-content">
    
    <div class="top-header d-flex align-items-center justify-content-between px-2 px-md-4 py-3 border-bottom">
        
        <button class="btn d-lg-none border-0 fs-3" id="sidebarToggle">
            <i class="bi bi-list"></i>
        </button>

        <div class="fw-semibold fs-5 ms-2">
            Quotation
        </div>

        <div class="flex-grow-1 d-flex justify-content-center px-4">
            <div class="search-wrapper position-relative w-100" style="max-width:520px;">
                <i class="bi bi-search position-absolute top-50 start-0 translate-middle-y ms-3 text-muted"></i>
                <input type="text" class="form-control ps-5 rounded-pill" placeholder="Search our marketplace">
            </div>
        </div>

        <div class="d-flex align-items-center gap-2 gap-md-4">
            <div class="position-relative d-flex align-items-center cursor-pointer cart-icon">
                <i class="bi bi-cart fs-5"></i>
                <span class="d-none d-md-inline ms-1">Cart</span>
                <span class="badge bg-danger rounded-pill position-absolute">01</span>
            </div>
            <div class="d-flex align-items-center cursor-pointer">
                <i class="bi bi-gear fs-5"></i>
                <span class="d-none d-md-inline ms-1">Settings</span>
            </div>
        </div>
    </div>

    <div class="top-bar d-flex flex-wrap align-items-center justify-content-between my-4">
        <div>
            <h4 class="mb-1">Broadcast Requests</h4>
            <p class="text-muted small">Track your Request and compare quotes from multiple suppliers</p>
        </div>
        <button class="btn add-new-request">
            <i class="bi bi-plus"></i> Add new request
        </button>
    </div>

    <div class="filters-wrapper">
        <div class="filter">
            <div class="filter-pill" data-filter="status">
                <span class="filter-label">Status:</span>
                <span class="filter-value" id="statusValue">All</span>
                <i class="bi bi-chevron-down"></i>
            </div>
            <div class="filter-dropdown">
                <div class="filter-option active" data-value="All">All</div>
                <div class="filter-option" data-value="Active">Active</div>
                <div class="filter-option" data-value="Expired">Expired</div>
            </div>
        </div>
        <div class="filter">
            <div class="filter-pill" data-filter="time">
                <span class="filter-label">Time:</span>
                <span class="filter-value" id="timeValue">This week</span>
                <i class="bi bi-chevron-down"></i>
            </div>
            <div class="filter-dropdown">
                <div class="filter-option active" data-value="This week">This week</div>
                <div class="filter-option" data-value="Today">Today</div>
            </div>
        </div>
    </div>

    <div class="card card-custom">
        <div class="table-responsive-custom">
            <table id="rfqTable" class="table table-striped table-hover nowrap m-0" style="width:100%">
                <thead>
                    <tr>
                        <th>Request ID</th>
                        <th>Title</th>
                        <th>Quantity</th>
                        <th>Type</th>
                        <th>Status</th>
                        <th>Request Date</th>
                        <th>Proposals</th>
                        <th>Actions</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>501</td>
                        <td>Sodium Benzoate</td>
                        <td>200 MT</td>
                        <td><span class="badge badge-broadcast">Broadcast</span></td>
                        <td><span class="badge badge-active">Active</span></td>
                        <td>2025-11-20</td>
                        <td>03</td>
                        <td>
                            <button class="btn btn-outline-dark btn-sm btn-view">View</button>
                            <span class="badge badge-new ms-1">New</span>
                        </td>
                    </tr>
                    <tr>
                        <td>500</td>
                        <td>Vitamin C (USP)</td>
                        <td>150 KG</td>
                        <td><span class="badge badge-broadcast">Broadcast</span></td>
                        <td><span class="badge badge-active">Active</span></td>
                        <td>2025-11-20</td>
                        <td>02</td>
                        <td><button class="btn btn-outline-dark btn-sm btn-view">View</button></td>
                    </tr>
                    <tr>
                        <td>499</td>
                        <td>Caustic Soda Flakes</td>
                        <td>500 MT</td>
                        <td><span class="badge badge-broadcast">Broadcast</span></td>
                        <td><span class="badge badge-active">Active</span></td>
                        <td>2025-11-20</td>
                        <td>01</td>
                        <td><button class="btn btn-outline-dark btn-sm btn-view">View</button></td>
                    </tr>
                    <tr>
                        <td>498</td>
                        <td>Hydrogen Peroxide</td>
                        <td>200 MT</td>
                        <td><span class="badge badge-broadcast">Broadcast</span></td>
                        <td><span class="badge badge-expired">Expired</span></td>
                        <td>2025-11-20</td>
                        <td>04</td>
                        <td><button class="btn btn-outline-dark btn-sm btn-view">View</button></td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</div>

<script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>
<script src="https://cdn.datatables.net/1.13.8/js/jquery.dataTables.min.js"></script>
<script src="https://cdn.datatables.net/1.13.8/js/dataTables.bootstrap5.min.js"></script>

<script>
    $(document).ready(function () {
        // Initialize DataTable
        $('#rfqTable').DataTable({
            responsive: false, // We use manual scroll container for better control
            pageLength: 5,
            lengthChange: false,
            searching: false
        });

        // Sidebar Toggle Logic
        $('#sidebarToggle, #closeSidebar, #sidebarOverlay').on('click', function() {
            $('#mainSidebar').toggleClass('show');
            $('#sidebarOverlay').toggleClass('active');
        });

        // Filter Dropdowns
        $('.filter-pill').on('click', function (e) {
            e.stopPropagation();
            $('.filter-dropdown').not($(this).next()).hide();
            $(this).next('.filter-dropdown').toggle();
        });

        $('.filter-option').on('click', function () {
            const value = $(this).data('value');
            const dropdown = $(this).closest('.filter-dropdown');
            const filter = dropdown.prev('.filter-pill');
            filter.find('.filter-value').text(value);
            dropdown.find('.filter-option').removeClass('active');
            $(this).addClass('active');
            dropdown.hide();
        });

        $(document).on('click', function () {
            $('.filter-dropdown').hide();
        });
    });
</script>

</body>
</html>