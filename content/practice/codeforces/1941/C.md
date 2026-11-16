---
title: "CF 1941C - Rudolf và sợi dây xấu xí"
description: "Chúng tôi được cung cấp một chuỗi và muốn xóa số ký tự tối thiểu để chuỗi kết quả không còn chứa \"pie\" hoặc \"map\" làm chuỗi con."
date: "2026-05-27T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1941
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 933 (Div. 3)"
rating: 900
weight: 1941
solve_time_s: 161
verified: true
draft: false
---
[CF 1941C - Rudolf và sợi dây xấu xí](https://codeforces.com/problemset/problem/1941/C) 

**Xếp hạng:** 900 
**Tags:** dp, tham lam, dây 
**Thời gian giải:** 2m 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi và muốn loại bỏ số ký tự tối thiểu để chuỗi kết quả không còn chứa`"pie"`hoặc`"map"`như một chuỗi con. 

Chi tiết quan trọng là việc xóa có thể xảy ra ở các vị trí tùy ý. Chúng tôi không sắp xếp lại các ký tự mà chỉ xóa một số ký tự. Mỗi lần xuất hiện của`"pie"`hoặc`"map"`phải bị phá vỡ bằng cách nào đó. 

Độ dài chuỗi trên tất cả các trường hợp thử nghiệm có thể đạt tới`10^6`, điều này ngay lập tức loại trừ mọi thứ bậc hai. MỘT`O(n^2)`giải pháp sẽ thực hiện xung quanh`10^12`trong trường hợp xấu nhất, vượt xa giới hạn. Chúng ta cần một cái gì đó gần với thời gian tuyến tính. 

Bản năng đầu tiên có thể là liên tục tìm kiếm`"pie"`Và`"map"`và xóa một ký tự khỏi mỗi lần xuất hiện. Điều đó hầu như có tác dụng, nhưng các mẫu chồng chéo tạo ra những tình huống phức tạp trong đó một thao tác xóa có thể phá hủy hai chuỗi con xấu cùng một lúc. 

Ví dụ kinh điển là:```
mapie
```Chuỗi này chứa cả`"map"`Và`"pie"`:```
mapie
^^^
  ^^^
```Một giải pháp tham lam bất cẩn có thể tính riêng và trả lời`2`. Nhưng xóa ký tự ở giữa`'p'`hủy cả hai chuỗi con cùng một lúc, vì vậy câu trả lời đúng là`1`. 

Một trường hợp cạnh khác xuất hiện khi nhiều chuỗi con xấu chỉ chồng lên nhau một phần. Coi như:```
mapmap
```hai`"map"`các lần xuất hiện là độc lập:```
mapmap
^^^
   ^^^
```Một lần xóa không thể phá vỡ cả hai lần, vì vậy câu trả lời là`2`. 

Dây rất ngắn cũng có vấn đề. Đối với đầu vào:```
p
```không thể có một chuỗi con có độ dài`3`, vậy câu trả lời là`0`. Triển khai kiểm tra một cách mù quáng`s[i:i+3]`việc xử lý không có giới hạn có thể mắc sai lầm ở đây. 

Trường hợp tinh tế cuối cùng là sự chồng chéo lặp đi lặp lại:```
pppiepieeee
```Các lần xuất hiện là:```
pppiepieeee
  ^^^
     ^^^
```Những điều này không trùng lặp một cách hữu ích, vì vậy chúng tôi thực sự cần xóa hai lần. 

## Phương pháp tiếp cận 

Chiến lược brute-force là thử mọi tập hợp xóa có thể và kiểm tra xem chuỗi kết quả có đẹp hay không. Điều này rõ ràng là đúng vì nó khám phá được mọi khả năng, nhưng nó hoàn toàn không khả thi. Ngay cả việc quyết định giữ hay loại bỏ mỗi ký tự cũng tạo ra`2^n`khả năng. 

Một cải tiến mạnh mẽ hợp lý hơn là liên tục quét chuỗi để tìm`"pie"`Và`"map"`, xóa một ký tự bất cứ khi nào tìm thấy một ký tự. Vì mỗi chuỗi con xấu có độ dài`3`, việc xóa bất kỳ ký tự nào của nó sẽ phá hủy sự xuất hiện đó. 

Vấn đề là xử lý chồng chéo. Nếu chúng ta tham lam xử lý`"map"`đầu tiên bên trong`"mapie"`, chúng tôi có thể xóa`'m'`hoặc`'a'`, thì vẫn có`"pie"`còn lại. Cuối cùng, chúng tôi sử dụng hai thao tác xóa thay vì một. 

Cái nhìn sâu sắc quan trọng là sự chồng chéo hữu ích duy nhất giữa`"map"`Và`"pie"`là mẫu đặc biệt:```
mapie
```Bên trong mẫu này, chỉ cần xóa một lần là đủ để hủy cả hai chuỗi con xấu. Cụ thể là loại bỏ phần trung tâm`'p'`hoạt động. 

Ngoài trường hợp này, mỗi lần xuất hiện của`"map"`hoặc`"pie"`phải đóng góp ít nhất một lần xóa một cách độc lập. 

Vì vậy, chiến lược tối ưu trở thành: 

1. Quét chuỗi từ trái sang phải. 
2. Trước tiên hãy kiểm tra xem`"mapie"`bắt đầu ở vị trí hiện tại. 
3. Nếu có, hãy tính một lần xóa và bỏ qua toàn bộ mẫu. 
4. Nếu không, hãy kiểm tra xem`"map"`hoặc`"pie"`bắt đầu ở đó. 
5. Nếu có, hãy tính một lần xóa và bỏ qua chuỗi con đó. 

Điều này hoạt động vì`"mapie"`là sự trùng lặp duy nhất mà việc xóa một lần sẽ tiết kiệm công việc so với việc xử lý hai mẫu riêng biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Quét tuyến tính tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm câu trả lời`0`. 

Biến này lưu trữ số lần xóa tối thiểu cần thiết. 
2. Quét chuỗi từ trái sang phải bằng chỉ mục`i`. 

Chúng tôi kiểm tra các chuỗi con bắt đầu ở mỗi vị trí. 
3. Trước tiên hãy kiểm tra xem chuỗi con bắt đầu tại`i`là`"mapie"`. 

Đây là trường hợp chồng chéo quan trọng. Nếu chúng tôi xử lý`"map"`hoặc`"pie"`đầu tiên, chúng ta có thể vô tình sử dụng hai thao tác xóa thay vì một. 
4. Nếu`"mapie"`được tìm thấy, tăng câu trả lời lên`1`và di chuyển`i`chuyển tiếp bởi`5`. 

Một lần xóa sẽ phá hủy cả hai chuỗi con xấu và không có ký tự nào trong khối này cần được xem xét lại. 
5. Ngược lại, hãy kiểm tra xem chuỗi con bắt đầu tại`i`là`"map"`hoặc`"pie"`. 

Mỗi lần xuất hiện như vậy yêu cầu phải xóa một lần. 
6. Nếu tìm thấy một trong hai chuỗi con, hãy tăng câu trả lời lên`1`và di chuyển`i`chuyển tiếp bởi`3`. 

Sau khi xóa một ký tự khỏi chuỗi con này, toàn bộ lần xuất hiện sẽ trở nên vô hại. 
7. Nếu không có chuỗi con xấu bắt đầu tại`i`, chuyển sang ký tự tiếp theo. 
8. Sau khi quá trình quét kết thúc, hãy xuất ra câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    s = input().strip()

    ans = 0
    i = 0

    while i < n:
        if i + 4 < n and s[i:i + 5] == "mapie":
            ans += 1
            i += 5
        elif i + 2 < n and (s[i:i + 3] == "map" or s[i:i + 3] == "pie"):
            ans += 1
            i += 3
        else:
            i += 1

    print(ans)
```Việc triển khai trực tiếp theo sau quá trình quét tham lam được mô tả trước đó. 

Điều kiện đầu tiên kiểm tra`"mapie"`trước bất cứ điều gì khác. Thứ tự đó quan trọng. Nếu chúng tôi kiểm tra`"map"`đầu tiên, sau đó`"mapie"`sẽ được tính không chính xác là hai lần xóa. 

Việc kiểm tra giới hạn cũng rất quan trọng:```
i + 4 < n
```đảm bảo rằng`s[i:i+5]`có giá trị để kiểm tra`"mapie"`. 

Tương tự:```
i + 2 < n
```bảo vệ việc kiểm tra chuỗi con có độ dài 3. 

Bỏ qua phía trước`5`hoặc`3`tránh xử lý lại các ký tự đã thuộc về chuỗi con xấu được xử lý. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
mmapnapie
```| tôi | Lát hiện tại | Hành động | Trả lời | 
| --- | --- | --- | --- | 
| 0 | mẹ | không có gì | 0 | 
| 1 | bản đồ | thành lập`"map"`| 1 | 
| 4 | ngủ trưa | không có gì | 1 | 
| 5 | api | không có gì | 1 | 
| 6 | bánh | thành lập`"pie"`| 2 | 

Câu trả lời cuối cùng:```
2
```Ví dụ này cho thấy hai chuỗi con xấu hoàn toàn riêng biệt. Mỗi yêu cầu xóa riêng của nó. 

### Ví dụ 2 

đầu vào:```
mapie
```| tôi | Lát hiện tại | Hành động | Trả lời | 
| --- | --- | --- | --- | 
| 0 | bản đồ | thành lập`"mapie"`| 1 | 

Câu trả lời cuối cùng:```
1
```Dấu vết này thể hiện trường hợp chồng chéo đặc biệt. điều trị`"map"`Và`"pie"`độc lập sẽ sản xuất không chính xác`2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý nhiều nhất một lần | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Tổng kích thước đầu vào tối đa là`10^6`, do đó, quét tuyến tính dễ dàng đủ nhanh trong giới hạn 2 giây. Việc sử dụng bộ nhớ không đổi bất kể độ dài chuỗi. 

## Trường hợp thử nghiệm 

### Trường hợp thử nghiệm 1 

đầu vào:```
1
1
a
```Sản lượng dự kiến:```
0
```Điều này xác minh trường hợp độ dài chuỗi tối thiểu. 

### Trường hợp thử nghiệm 2 

đầu vào:```
1
5
mapie
```Sản lượng dự kiến:```
1
```Điều này kiểm tra tối ưu hóa chồng chéo quan trọng. 

### Trường hợp thử nghiệm 3 

đầu vào:```
1
6
mapmap
```Sản lượng dự kiến:```
2
```Điều này xác nhận rằng các lần xuất hiện độc lập được tính riêng. 

### Trường hợp thử nghiệm 4 

đầu vào:```
1
9
piepiepie
```Sản lượng dự kiến:```
3
```Điều này nhấn mạnh các chuỗi con xấu không chồng chéo lặp đi lặp lại. 

## Vỏ cạnh 

Trường hợp khó khăn đầu tiên là mẫu chồng chéo:```
mapie
```Thuật toán kiểm tra`"mapie"`trước`"map"`hoặc`"pie"`. Tại chỉ mục`0`, nó phát hiện sự chồng chéo hoàn toàn và chỉ thêm một lần xóa. Sau đó, nó bỏ qua tất cả năm ký tự. Câu trả lời cuối cùng trở thành`1`, đó là tối ưu. 

Bây giờ hãy xem xét các lần xuất hiện độc lập:```
mapmap
```Tại chỉ mục`0`, thuật toán tìm`"map"`và tăng câu trả lời cho`1`. Nó bỏ qua ba vị trí và hạ cánh ở vị trí thứ hai`"map"`, yêu cầu xóa khác. Kết quả là`2`. 

Đối với một chuỗi rất ngắn:```
p
```các điều kiện:```
i + 4 < n
i + 2 < n
```cả hai đều thất bại ngay lập tức, do đó không có hoạt động kiểm tra chuỗi con nào xảy ra. Câu trả lời vẫn còn`0`. 

Cuối cùng, kiểm tra các mẫu lặp lại:```
pppiepieeee
```Quá trình quét đạt đến điểm đầu tiên`"pie"`và tính một lần xóa. Sau khi bỏ qua phía trước, nó đến thứ hai`"pie"`và đếm cái khác. Vì những lần xuất hiện này không trùng nhau nên câu trả lời đúng là`2`.
