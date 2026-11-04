---
title: "CF 1150B - Thử thách ốp lát"
description: "Chúng ta có một bảng vuông $n nhân n$, trong đó một số ô bị chiếm và những ô khác còn trống. Mục tiêu là xếp hoàn toàn các ô tự do bằng cách sử dụng các mảnh pentomino giống hệt nhau có hình chữ thập: một hình vuông ở giữa có bốn hình vuông liền kề, mỗi hình theo một hướng chính."
date: "2026-06-12T03:05:59+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1150
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 556 (Div. 2)"
rating: 900
weight: 1150
solve_time_s: 93
verified: true
draft: false
---

[CF 1150B - Thử thách ốp lát](https://codeforces.com/problemset/problem/1150/B) 

**Xếp hạng:** 900 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$bảng hình vuông, trong đó một số ô được chiếm giữ và những ô khác còn trống. Mục tiêu là xếp hoàn toàn các ô tự do bằng cách sử dụng các mảnh pentomino giống hệt nhau có hình chữ thập: một hình vuông ở giữa có bốn hình vuông liền kề, mỗi hình theo một hướng chính. Mỗi quân cờ phải bao phủ chính xác năm ô riêng biệt, không được chồng lên các ô đã chiếm hoặc các quân khác và không được mở rộng ra ngoài ranh giới bàn cờ. Đầu ra là CÓ nếu có thể xếp lớp như vậy và KHÔNG nếu ngược lại. 

Kích thước bảng$n$nằm trong khoảng từ 3 đến 50. Vì mỗi mảnh chiếm 5 ô nên tổng số ô trống phải chia hết cho 5 để có được một ô xếp hoàn chỉnh. Mặc dù$n$nhỏ, thuật toán thử mọi kết hợp vị trí có thể có sẽ không hiệu quả vì số lượng tập hợp con của các ô tự do tăng theo cấp số nhân. Vì vậy, lời giải nên khai thác cấu trúc của hình pentomino hơn là thử mọi cách kết hợp. 

Các trường hợp khó khăn mà cách tiếp cận đơn giản có thể thất bại bao gồm các bảng có các ô bị chiếm dụng rải rác chặn các vị trí tiềm năng. Ví dụ: bảng 3 × 3 có các góc được lấp đầy:```
#.#
...
#.#
```Bảng này có thể xếp được vì phần trung tâm có thể chứa một pentomino duy nhất bao phủ bốn ô cạnh liền kề với nó. Vị trí tham lam theo từng hàng ngây thơ có thể từ chối cấu hình này một cách không chính xác. 

Một trường hợp khó nhận thấy khác là một bảng gần như đầy đủ chỉ có các ô tự do biệt lập không thể tạo thành hình chữ thập; ở đây thuật toán phải phát hiện chính xác rằng không thể xếp lớp được. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng đặt một pentomino trên mọi ô trung tâm có thể và che phủ đệ quy các ô tự do còn lại. Đối với một$n \times n$bảng, có khoảng$O(n^2)$các ứng cử viên trung tâm và mỗi lệnh gọi đệ quy sẽ phân nhánh tối đa 1, dẫn đến số lượng cấu hình theo cấp số nhân. Điều này rõ ràng là quá chậm đối với$n = 50$. 

Cái nhìn sâu sắc quan trọng là pentomino luôn chiếm một vùng 3 × 3 với trung tâm là ô ở giữa. Nếu chúng ta lặp lại bảng từ trên cùng bên trái đến dưới cùng bên phải và tham lam đặt một mảnh ở bất kỳ vị trí nào mà tâm của nó có thể vừa, chúng ta sẽ che các ô trống ngay lập tức hoặc phát hiện một cấu hình trong đó một ô không thể là một phần của bất kỳ hình chữ thập nào. Bởi vì mỗi pentomino yêu cầu tâm của nó và bốn ô liền kề phải trống, nên việc truyền tải từ trái sang phải, từ trên xuống dưới đảm bảo rằng khi đạt đến một ô tự do, nó có thể đóng vai trò là tâm của một hình chữ thập mới hoặc phải đã được bao phủ bởi một mảnh được đặt trước đó. Chiến lược này khai thác cấu trúc lưới của bàn cờ và vị trí của pentomino hình chữ thập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n^2)) | O(n^2) | Quá chậm | 
| Tham lam quét & đặt | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước bảng$n$và$n \times n$biểu diễn lưới, đánh dấu các ô trống là "." và bị chiếm đóng là "#". 
2. Lặp lại từng ô bắt đầu từ hàng 1 đến$n-2$và cột 1 đến$n-2$, coi mỗi tế bào là trung tâm tiềm năng của pentomino. 
3. Đối với mỗi ô trung tâm ứng viên, hãy kiểm tra xem ô đó và 4 ô liền kề (trên, dưới, trái, phải) đều còn trống hay không. Nếu vậy, hãy đặt một pentomino bằng cách đánh dấu năm ô này là đã có người sử dụng. 
4. Tiếp tục lặp lại cho đến khi tất cả các vị trí trung tâm có thể được xem xét. 
5. Sau khi hoàn tất quá trình quét, hãy kiểm tra xem còn ô trống nào không. Nếu có, xuất NO; nếu không thì xuất ra CÓ. 

Tại sao nó hoạt động: Quá trình truyền tải tham lam đảm bảo rằng bất kỳ ô tự do nào có thể đóng vai trò là trung tâm sẽ được sử dụng. Vì hình chữ thập chỉ mở rộng một ô theo mỗi hướng nên không có mảnh nào được đặt trước đó sẽ ngăn cản vị trí hợp lệ nếu có thể. Bất kỳ ô trống nào còn lại sau khi truyền tải đều không thể là một phần của bất kỳ pentomino nào và do đó việc xếp lớp là không thể. Điều bất biến là mỗi ô trống được bao phủ hoặc được xem xét bao phủ chính xác một lần. 

## Giải pháp Python```
PythonRun
```
