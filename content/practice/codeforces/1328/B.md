---
problem: 1328B
contest_id: 1328
problem_index: B
name: "K-th Beautiful String"
contest_name: "Codeforces Round 629 (Div. 3)"
rating: 1300
tags: ["binary search", "brute force", "combinatorics", "implementation", "math"]
answer: passed_samples
verified: false
solve_time_s: 231
date: 2026-06-14
model: gpt-5-5
samples_passed: 0
samples_total: 0
share_url: https://chatgpt.com/share/6a2dfc6a-d998-83ec-acae-ab74293c87ce
---

#CF 1328B - Dây Đẹp thứ K 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, tổ hợp, thực hiện, toán học 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 3 phút 51 giây 
**Đã xác minh:** không (0/0 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2dfc6a-d998-83ec-acae-ab74293c87ce 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi đang xem xét tất cả các chuỗi nhị phân có độ dài`n`chứa chính xác hai lần xuất hiện của chữ cái`b`, trong khi mọi vị trí khác đều được lấp đầy`a`. Vì bảng chữ cái được sắp xếp theo thứ tự`a < b`, chuỗi bắt đầu bằng nhiều hàng đầu hơn`a`s đến sớm hơn theo thứ tự từ điển và đặt một`b`trước đẩy một chuỗi sau. 

Nhiệm vụ là tưởng tượng tất cả các chuỗi như vậy được sắp xếp giống như các từ trong từ điển và trả về vị trí của chuỗi đó.`k`. 

Đầu vào cung cấp nhiều truy vấn. Mỗi truy vấn cung cấp một độ dài`n`và một thứ hạng`k`và chúng ta phải trực tiếp xây dựng chuỗi thứ k mà không tạo ra tất cả các ứng cử viên. 

Các ràng buộc buộc chúng tôi phải đạt tổng số O(n) cho mỗi thử nghiệm hoặc giải pháp tốt hơn. Vì tổng của tất cả`n`tùy thuộc vào`10^5`, bất kỳ cách tiếp cận nào là phương pháp bậc hai trong`n`hoặc liệt kê tất cả các cặp`b`vị trí cho mỗi trường hợp thử nghiệm là không khả thi. 

Một nỗ lực ngây thơ sẽ tạo ra tất cả các cặp vị trí`(i, j)`với`i < j`, xây dựng chuỗi và sắp xếp chúng. Điều đó đã tạo ra`O(n^2)`các chuỗi cho mỗi trường hợp thử nghiệm và thậm chí việc xây dựng chúng một cách rõ ràng sẽ dẫn đến TLE. 

Một trường hợp thất bại tinh vi hơn xuất phát từ việc cố gắng mô phỏng thứ tự từ điển một cách tham lam mà không hiểu cấu trúc tổ hợp. Ví dụ, giả sử sai lần đầu tiên`b`vị trí được xác định độc lập với vị trí thứ hai dẫn đến xếp hạng sai vì số lần hoàn thành phụ thuộc vào cả hai lựa chọn. 

## Phương pháp tiếp cận 

Chế độ xem brute-force rất đơn giản: chọn hai chỉ số cho`b`ký tự, xây dựng chuỗi và sắp xếp tất cả kết quả. Điều này hoạt động về mặt khái niệm vì mỗi chuỗi hợp lệ tương ứng duy nhất với một cặp`(i, j)`với`i < j`. Tuy nhiên số lượng các cặp như vậy là`n(n-1)/2`, do đó, ngay cả việc liệt kê chúng cũng trở thành bậc hai và việc sắp xếp chúng sẽ thêm một hệ số logarit khác lên trên cấu trúc chuỗi nặng. 

Điều quan trọng là thứ tự từ điển phù hợp hoàn hảo với vị trí đầu tiên của`b`. Nếu chúng ta sửa cái đầu tiên`b`ở vị trí`i`, sau đó là thứ hai`b`có thể được đặt ở bất cứ đâu một cách nghiêm ngặt sau`i`. Tất cả các chuỗi như vậy tạo thành một khối liền kề theo thứ tự từ điển. 

Đối với một cố định`i`, số cách chọn thứ hai`b`là`n - i`. Điều đó có nghĩa là kích thước khối cho vị trí`i`là`n - i`. Chúng ta có thể quét các khối này theo thứ tự tăng dần`i`và trừ kích thước khối từ`k`cho đến khi chúng ta tìm được cái đúng đầu tiên`b`chức vụ. Sau đó, phần bù còn lại trực tiếp cho chúng ta biết vị trí thứ hai`b`đi. 

Điều này làm giảm vấn đề xuống còn một lần quét tuyến tính cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² log n) | O(n²) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mọi chuỗi hợp lệ được xác định bởi hai chỉ số`i < j`trong đó cả hai vị trí đều chứa`b`. 

1. Chúng tôi lặp lại`i`từ 1 đến`n-1`, điều trị`i`như vị trí của người đầu tiên`b`. 
2. Đối với mỗi`i`, chúng tôi tính toán có bao nhiêu chuỗi bắt đầu bằng`b`Tại`i`. Điều này bằng`n - i`bởi vì thứ hai`b`có thể ở bất kỳ vị trí nào sau`i`. 
3. Nếu`k`lớn hơn kích thước khối này, chúng tôi trừ nó đi và chuyển sang kích thước khối tiếp theo`i`. Điều này có nghĩa là chuỗi thứ k không có trong khối này. 
4. Một khi chúng tôi tìm thấy một`i`như vậy`k <= n - i`, chúng tôi sửa cái đầu tiên`b`Tại`i`. 
5. Thứ hai`b`lúc đó đang ở vị trí`i + k`. Điều này có hiệu quả vì trong khối này, thứ tự từ điển tương ứng chính xác với vị trí tăng dần của từ thứ hai.`b`. 
6. Chúng tôi xây dựng chuỗi cuối cùng với`a`ở khắp mọi nơi
