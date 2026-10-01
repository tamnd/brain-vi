---
title: "CF 104871E - Lịch trình bình đẳng"
description: "Chúng tôi được cung cấp hai lịch trình hoàn chỉnh mô tả phạm vi phủ sóng theo cuộc gọi liên tục theo dòng thời gian bắt đầu từ thời điểm 0. Mỗi lịch trình là một phân vùng của khoảng thời gian thành các phân đoạn liên tiếp, không chồng chéo."
date: "2026-06-28T10:37:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104871
codeforces_index: "E"
codeforces_contest_name: "2023-2024 ICPC Central Europe Regional Contest (CERC 23)"
rating: 0
weight: 104871
solve_time_s: 34
verified: false
draft: false
---

[CF 104871E - Lịch trình bình đẳng](https://codeforces.com/problemset/problem/104871/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai lịch trình hoàn chỉnh mô tả phạm vi phủ sóng theo cuộc gọi liên tục theo dòng thời gian bắt đầu từ thời điểm 0. Mỗi lịch trình là một phân vùng của khoảng thời gian thành các phân đoạn liên tiếp, không chồng chéo. Mỗi phân đoạn chỉ định một người chịu trách nhiệm cho toàn bộ khoảng thời gian đó. 

Nhiệm vụ là so sánh tổng thời gian mỗi người trực trong lịch trình đầu tiên với lịch trình thứ hai. Đối với mỗi người xuất hiện trong ít nhất một lịch trình, chúng tôi tính toán sự khác biệt: thời gian trong lịch trình thứ hai trừ đi thời gian trong lịch trình đầu tiên. Chỉ những khác biệt khác 0 mới được báo cáo, sắp xếp theo tên. 

Cấu trúc của mỗi lịch trình là đặc biệt quan trọng. Vì các phân đoạn liền kề nhau và không chồng chéo nên mỗi lịch trình có thể được hiểu là phạm vi bao phủ đầy đủ của khoảng thời gian từ 0 đến điểm cuối cuối cùng, không có khoảng trống hoặc chồng chéo. Điều này loại bỏ bất kỳ sự mơ hồ nào về phạm vi bảo hiểm một phần hoặc tính hai lần. 

Ràng buộc mà mỗi lịch trình kết thúc nhiều nhất là 1000 có nghĩa là tổng số lần thay đổi đơn vị thời gian là nhỏ. Ngay cả khi chúng tôi rời rạc hóa thời gian ở độ phân giải đơn vị, giải pháp dựa trên mảng trực tiếp vẫn khả thi vì dòng thời gian ngắn. 

Trường hợp phức tạp xảy ra khi một tên chỉ xuất hiện trong một lịch trình. Ví dụ: nếu ai đó có mặt trong lịch trình một nhưng vắng mặt trong lịch trình hai, thì sự khác biệt của họ sẽ âm trong toàn bộ thời gian của họ. Một trường hợp đặc biệt khác là khi lên lịch hoán đổi các bài tập nhưng vẫn giữ nguyên thời lượng, điều này sẽ không mang lại kết quả đầu ra. 

Một sai lầm ngây thơ là xử lý các phân đoạn một cách độc lập và quên tích lũy thời lượng cho mỗi người. Một vấn đề phổ biến khác là không thể thiết lập lại hoặc phân tách tích lũy giữa hai lịch trình, dẫn đến sự đóng góp lẫn lộn. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là mở rộng từng lịch trình thành các đơn vị thời gian riêng lẻ. Với mỗi khoảng si đến ei, chúng ta gán người ti cho tất cả các số nguyên trong khoảng đó. Chúng tôi duy trì hai mảng, một mảng cho mỗi lịch trình, ánh xạ từng đơn vị thời gian vào một tên. Sau khi mở rộng, chúng tôi đếm số lần mỗi tên xuất hiện trong mỗi mảng. 

Điều này đúng vì các lịch trình tạo thành một phân vùng thời gian rời rạc, do đó mỗi đơn vị thời gian chỉ thuộc về một người. Tuy nhiên, việc mở rộng các khoảng thời gian thành đơn vị thời gian sẽ không hiệu quả nếu dòng thời gian lớn, vì chi phí sẽ tỷ lệ thuận với tổng chiều dài của tất cả các khoảng thời gian. 

Trong vấn đề này, điểm cuối tối đa chỉ là 1000, do đó việc mở rộng vũ phu đã đủ nhanh. Nhưng chúng tôi vẫn có thể trình bày một giải pháp tối ưu rõ ràng hơn để tránh việc mở rộng rõ ràng và tổng hợp trực tiếp độ dài khoảng thời gian của mỗi người. 

Quan sát quan trọng là mỗi phân đoạn đã mã hóa một khoảng thời gian ei − si. Chúng ta có thể chỉ cần cộng giá trị này vào tổng số của người tương ứng. Thực hiện việc này riêng biệt cho lịch trình thứ nhất và lịch trình thứ hai sẽ cho chúng ta tổng số chính xác mà chúng ta cần theo thời gian tuyến tính về số lượng phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mở rộng đơn vị) | O(T) trong đó T ≤ 1000 | O(T + tên) | Đã chấp nhận | 
| Tổng hợp khoảng thời gian | O(N) | O(tên) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc từng dòng lịch trình đầu tiên và duy trì một từ điển ánh xạ từng tên theo thời gian tích lũy. Với mỗi đoạn si ei ti, hãy thêm ei − si vào mục tương ứng. 
2. Lặp lại quy trình tương tự cho lịch trình thứ hai bằng cách sử dụng một từ điển riêng. Việc giữ chúng tách biệt đảm bảo chúng tôi không bao giờ trộn lẫn các khoản đóng góp giữa các lịch trình. 
3. Xây dựng tập hợp tất cả các tên xuất hiện trong từ điển. Điều này đảm bảo chúng tôi tính đến những người chỉ xuất hiện trong một lịch trình. 
4. Với mỗi tên, hãy tính diff[name] = two[name] − first[name]. Điều này trực tiếp mã hóa cho dù
