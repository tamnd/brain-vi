---
title: "CF 1515C - Phượng Hoàng và Tháp"
description: "Chúng ta có một tập hợp gồm n khối, mỗi khối có chiều cao hi. Phoenix muốn xây dựng chính xác m tòa tháp bằng cách sử dụng tất cả các khối. Chiều cao của mỗi tòa tháp là tổng số khối của nó và cách sắp xếp \"đẹp\" yêu cầu không có hai tòa tháp nào có chiều cao khác nhau quá x."
date: "2026-06-10T18:31:16+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1515
codeforces_index: "C"
codeforces_contest_name: "Codeforces Global Round 14"
rating: 1400
weight: 1515
solve_time_s: 226
verified: false
draft: false
---

[CF 1515C - Phượng hoàng và những tòa tháp](https://codeforces.com/problemset/problem/1515/C) 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 3 phút 46s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ`n`khối, mỗi khối có chiều cao`h_i`. Phoenix muốn xây dựng chính xác`m`tháp sử dụng tất cả các khối. Chiều cao của mỗi tòa tháp là tổng số khối của nó và sự sắp xếp "đẹp" đòi hỏi không có hai tòa tháp nào có chiều cao khác nhau quá`x`. Nhiệm vụ là gán mỗi khối cho một tòa tháp sao cho đáp ứng được đặc tính này. Nếu có thể, chúng ta phải tạo ra một chuỗi cho biết mỗi khối thuộc về tòa tháp nào; nếu không, chúng tôi in`NO`. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp số khối, số lượng tháp, chênh lệch chiều cao tối đa cho phép`x`và chiều cao của tất cả các khối. Đầu ra phải là`YES`tiếp theo là việc gán các khối cho các tòa tháp, hoặc`NO`. 

Các ràng buộc cho chúng ta biết rằng tổng số khối trong tất cả các trường hợp thử nghiệm lên tới 100.000. Với giới hạn thời gian 2 giây, điều này có nghĩa là chúng ta cần một thuật toán gần như tuyến tính theo`n`mỗi trường hợp thử nghiệm. Các vòng lặp lồng nhau hoặc phương pháp tiếp cận O(n²) quá chậm. 

Trường hợp cạnh tinh tế xảy ra khi tất cả các khối có cùng chiều cao. Một nhiệm vụ tham lam ngây thơ luôn lấp đầy tòa tháp đầu tiên trước khi chuyển sang tòa tháp tiếp theo có thể tạo ra cấu hình không cân bằng, vượt quá mức cho phép.`x`sự khác biệt, mặc dù cách tiếp cận xen kẽ sẽ có hiệu quả. Ví dụ, nếu`n=3, m=2, x=1`với độ cao`[1,1,1]`, việc gán tất cả các khối cho tháp đầu tiên sẽ mang lại độ cao`[3,0]`với chênh lệch 3, lớn hơn`x`. Sự phân công chính xác`[1,2,1]`mang lại chiều cao tháp`[2,1]`, thỏa mãn ràng buộc. 

Một trường hợp cạnh khác xảy ra khi`m = n`, trong đó mỗi tháp phải chứa chính xác một khối. Giải pháp trong trường hợp này là không đáng kể nhưng thuật toán phải xử lý nó một cách chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực xem xét mọi khả năng phân bổ khối giữa các tòa tháp. Đối với mỗi kết hợp, chúng tôi tính toán độ cao của tháp và kiểm tra xem chênh lệch tối đa có lớn nhất không`x`. Số cách phân vùng`n`khối vào`m`tháp tăng theo cấp số nhân, khoảng O(m^n). Điều này là không thể ngay cả đối với nhỏ`n`. 

Quan sát chính là giới hạn chiều cao`x`ít hạn chế hơn so với vẻ ngoài của nó. Vì mỗi chiều cao khối`h_i ≤ x`, sự chênh lệch tuyệt đối về độ cao giữa hai khối bất kỳ lớn nhất là`x`. Điều này cho phép thực hiện một chiến lược tham lam đơn giản: luôn đặt khối tiếp theo lên tòa tháp hiện tại ngắn nhất. Bằng cách duy trì các tòa tháp trong hàng ưu tiên được sắp xếp theo chiều cao hiện tại, chúng tôi đảm bảo rằng các tòa tháp vẫn cân bằng khi các khối được chỉ định. Việc sắp xếp các khối là không thực sự cần thiết, nhưng việc chỉ định các khối cao hơn sớm hơn có thể làm giảm sự tăng đột biến tạm thời về chiều cao của tháp. Sau khi tất cả các khối được chỉ định, chiều cao tháp thu được được đảm bảo có chênh lệch không quá`x`, bởi vì không có khối nào vượt quá`x`. 

Điều này làm giảm giải pháp từ thời gian hàm mũ xuống O(n log m), vì chúng tôi duy trì một đống kích thước`m`vì`n`phần chèn thêm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m^n) | O(n) | Quá chậm | 
| Đống tham lam | O(n log m) | O(m+n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một đống tháp tối thiểu với chiều cao hiện tại được đặt thành 0, cùng với các chỉ số từ 1 đến m. Điều này cho phép truy xuất hiệu quả tháp ngắn nhất tại bất kỳ điểm nào. 
2. Lặp lại các khối theo thứ tự đầu vào. Đối với mỗi khối, hãy bật tháp có chiều cao hiện tại nhỏ nhất trong đống. 
3. Gán khối cho tháp này, cập nhật tổng chiều cao của tháp bằng cách cộng chiều cao của khối. Ghi chỉ số tháp vào một mảng khớp với thứ tự khối. 
4. Đẩy tháp đã cập nhật trở lại đống. 
5. Sau khi đã gán tất cả các khối, hãy in`YES`theo sau là mảng gán. Nhiệm vụ đảm bảo rằng chênh lệch chiều cao giữa hai tòa tháp bất kỳ không thể vượt quá`x`bởi vì không có khối nào cao hơn`x`và chúng tôi luôn đặt các khối trên tòa tháp ngắn nhất hiện nay. 

### Tại sao nó hoạt động 

Điều bất biến là sự khác biệt giữa tháp cao nhất và tháp ngắn nhất được giảm thiểu ở mỗi bước. Vì mỗi khối có chiều cao tối đa là`x`, không có sự bổ sung đơn lẻ nào có thể làm tăng sự khác biệt vượt quá`x`trong một bước. Việc đặt các khối cao nhất lên trước hoặc duy trì trật tự không ảnh hưởng đến tính chính xác mà chỉ ảnh hưởng đến sự dao động tạm thời của độ cao. Heap đảm bảo rằng các khối luôn được gán cho tháp tối ưu để giảm thiểu sự khác biệt. 

## Giải pháp Python```
PythonRun
```Trước tiên, giải pháp sẽ đọc số lượng trường hợp kiểm thử và xử lý từng trường hợp kiểm thử một cách độc lập. Một cửa hàng nhỏ`(current height, tower index)`bộ dữ liệu, đảm bảo tháp ngắn nhất luôn có sẵn trong thời gian O(log m). Các khối được gán tuần tự theo thứ tự đầu vào. Mảng gán lưu trữ tháp mà mỗi khối thuộc về và được in sau khi xử lý tất cả các khối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Khối đầu vào`[1,2,3,1,2]`với`m=2`. 

| Bước | Trạng thái heap (chiều cao, tháp) | Chặn | Bài tập | Đống sau khi đẩy | 
| --- | --- | --- | --- | --- | 
| 1 | [(0,1),(0,2)] | 1 | 1 | [(0,2),(1,1)] | 
| 2 | [(0,2),(1,1)] | 2 | 2 | [(1,1),(2,2)] | 
| 3 | [(1,1),(2,2)] | 3 | 1 | [(2,2),(4,1)] | 
| 4 | [(2,2),(4,1)] | 1 | 2 | [(3,2),(4,1)] | 
| 5 | [(3,2),(4,1)] | 2 | 2 | [(5,2),(4,1)] | 

Nhiệm vụ cuối cùng:`[1,2,1,2,2]`. Chiều cao tháp`[4,5]`, sự khác biệt`1 ≤ x=3`. 

### Ví dụ 2 

Khối đầu vào`[1,1,2,3]`với`m=3`. 

| Bước | Trạng thái đống | Chặn | Bài tập | Đống sau khi đẩy | 
| --- | --- | --- | --- | --- | 
| 1 | [(0,1),(0,2),(0,3)] | 1 | 1 | [(0,2),(0,3),(1,1)] | 
| 2 | [(0,2),(0,3),(1,1)] | 1 | 2 | [(0,3),(1,1),(1,2)] | 
| 3 | [(0,3),(1,1),(1,2)] | 2 | 3 | [(1,1),(1,2),(2,3)] | 
| 4 | [(1,1),(1,2),(2,3)] | 3 | 1 | [(2,1),(2,3),(1,2)] | 

Nhiệm vụ cuối cùng:`[1,2,3,1]`. Chiều cao tháp`[4,1,2]`, sự khác biệt`3 ≤ x=3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log m) | Mỗi lần chèn khối và cập nhật vùng heap mất O(log m), tổng cộng n khối. | 
| Không gian | O(m+n) | Heap lưu trữ m tháp, mảng trả lời lưu trữ n bài tập. | 

Cho n ≤ 10^5 và m ≤ n, giải pháp thực hiện dễ dàng trong vòng 2 giây. 

## Trường hợp thử nghiệm```
PythonRun
```
