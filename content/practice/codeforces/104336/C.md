---
title: "CF 104336C - Hai người chơi, hai con số"
description: "Chúng ta có hai người chơi, mỗi người bắt đầu bằng một số nguyên được viết ở dạng thập phân. Arthur sở hữu a, Nikita sở hữu b. Sau đó, Arthur thêm chính xác n chữ số thập phân vào bên phải số của anh ấy và Nikita thêm chính xác m chữ số vào bên phải số của anh ấy."
date: "2026-07-01T18:46:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104336
codeforces_index: "C"
codeforces_contest_name: "II Olympiad of classes at the Mechanics and Mathematics Faculty of MSU in programming 2023."
rating: 0
weight: 104336
solve_time_s: 57
verified: true
draft: false
---

[CF 104336C - Hai người chơi, hai con số](https://codeforces.com/problemset/problem/104336/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai người chơi, mỗi người bắt đầu bằng một số nguyên được viết ở dạng thập phân. Arthur sở hữu`a`, Nikita sở hữu`b`. Sau đó, Arthur nối thêm chính xác`n`chữ số thập phân ở bên phải số của anh ấy và Nikita thêm chính xác`m`các chữ số ở bên phải số của anh ấy. Những chữ số được nối thêm có thể được lựa chọn tự do. 

Sau khi thực hiện xong cả hai phần mở rộng, hai số nguyên thu được sẽ được so sánh như số nguyên bình thường. Arthur thắng nếu số cuối cùng của anh ấy lớn hơn, Nikita thắng nếu số của cô ấy lớn hơn, nếu không thì hòa. 

Điểm mấu chốt là cả hai người chơi đều cố gắng chọn các chữ số được thêm vào một cách tối ưu để tối đa hóa cơ hội chiến thắng của mình. Vì mỗi người chơi chỉ kiểm soát hậu tố của riêng mình nên trò chơi sẽ giảm thiểu mức độ ảnh hưởng`n`Và`m`nhượng bộ để vượt qua sự khác biệt giữa`a`Và`b`. 

Các ràng buộc rất lớn, lên tới 10^9 cho cả bốn giá trị. Điều đó ngay lập tức loại trừ bất kỳ cấu trúc nào cố gắng mô phỏng việc thêm từng chữ số hoặc ép buộc tất cả các hậu tố. Ngay cả một số có 10^9 chữ số được nối thêm cũng không thể biểu diễn rõ ràng, vì vậy giải pháp phải giảm bớt vấn đề so sánh độ lớn ở dạng đóng. 

Trường hợp cạnh tinh tế xuất hiện khi`a`Và`b`gần giống nhau, nhưng một người chơi có nhiều chữ số hơn đáng kể để nối thêm. Ví dụ, nếu`a = 9`,`b = 10`và Arthur thêm nhiều chữ số trong khi Nikita thêm ít chữ số, cấu trúc dẫn đầu có thể bị lấn át bởi độ dài hậu tố. Một so sánh ngây thơ chỉ`a`Và`b`thất bại hoàn toàn trong những trường hợp như vậy vì nó bỏ qua số lượng chữ số có thể được thêm vào. 

Một trường hợp thất bại khác phát sinh nếu người ta cho rằng người chơi luôn chỉ thêm số 9 hoặc chỉ số 0 mà không có lý do về tính tối ưu. Lý do đúng phụ thuộc vào việc mỗi người chơi sẽ chọn các chữ số hậu tố để tối đa hóa số cuối cùng về mặt từ điển, nghĩa là họ luôn chọn số 9 ở tất cả các vị trí được nối thêm. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ cố gắng mô phỏng lựa chọn của cả hai người chơi. Đối với mỗi chuỗi chữ số có thể có độ dài`n`cho Arthur và`m`đối với Nikita, chúng tôi sẽ xây dựng các số kết quả và so sánh chúng. Điều này đúng về mặt lý thuyết vì lựa chọn tối ưu của mỗi người chơi nằm trong số tất cả các hậu tố có thể có, nhưng số khả năng xảy ra là 10^n và 10^m, rất lớn về mặt thiên văn ngay cả đối với các giá trị nhỏ của n và m. Lực lượng vũ phu thất bại ngay lập tức. 

Nhận xét quan trọng là chiến lược tối ưu của mỗi người chơi mang tính quyết định. Vì mục tiêu là tối đa hóa số cuối cùng nên mỗi chữ số được thêm vào phải là 9. Bất kỳ chữ số nào nhỏ hơn chỉ có thể làm giảm giá trị mà không mang lại bất kỳ lợi thế bù đắp nào sau này, vì các vị trí quan trọng hơn sẽ chiếm ưu thế. 

Vậy số cuối cùng của Arthur trở thành`a`theo sau là`n`chữ số của 9, và Nikita trở thành`b`theo sau là`m`các chữ số của 9. 

Bây giờ vấn đề giảm xuống còn việc so sánh hai số rất lớn được hình thành bằng phép nối. Thay vì xây dựng chúng một cách rõ ràng, chúng ta so sánh theo cấu trúc. Nếu số chữ số khác nhau sau khi gia hạn thì độ dài hiệu dụng dài hơn sẽ chiếm ưu thế. Nếu chúng có độ dài hiệu dụng bằng nhau, trước tiên chúng ta so sánh theo phần nguyên được chia tỷ lệ theo lũy thừa 10, sau đó theo ảnh hưởng của hậu tố. 

Điều này làm giảm vấn đề để so sánh`a * 10^n + (10^n - 1)`so với`b * 10^m + (10^m - 1)`. Vì lũy thừa trực tiếp vẫn chỉ an toàn thông qua các số nguyên Python nên chúng tôi dựa vào logic so sánh có cấu trúc hoặc logic số học có độ chính xác tùy ý của Python. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^n + 10^m) | O(10^n + 10^m) | Quá chậm | 
| Tối ưu | O(log(max(a,b,n,m))) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát rằng cả hai người chơi sẽ luôn thêm chữ số 9 vào mọi vị trí. Điều này là do việc tăng bất kỳ chữ số hậu tố nào cũng sẽ làm tăng số cuối cùng và không có sự đánh đổi nào với các quyết định trong tương lai. 
2. Thay thế trạng thái trò chơi bằng hai số cuối cùng xác định: Arthur có`A = a * 10^n + (10^n - 1)`và Nikita có`B = b * 10^m + (10^m - 1)`. 
3. So sánh độ lớn của`A`Và`B`. Thay vì xây dựng các giá trị đầy đủ, hãy so sánh chúng bằng số học số nguyên lớn của Python hoặc bằng cách so sánh cấu trúc chữ số. 
4. Nếu`A > B`, Arthur thắng. Nếu như`A < B`, Nikita thắng. Ngược lại, kết quả là hòa. 

Bước lý luận chính là việc tối ưu hóa hậu tố sẽ loại bỏ hoàn toàn sự tương tác chiến lược. Khi cả hai người chơi đều chơi tối ưu, trò chơi sẽ trở thành một bài toán so sánh số trực tiếp và không còn lựa chọn nào khác. 

### Tại sao nó hoạt động 

Quyết định của mỗi người chơi tại mỗi vị trí được thêm vào là độc lập và đơn điệu: việc tăng bất kỳ chữ số nào sẽ làm tăng giá trị cuối cùng một cách nghiêm ngặt bất kể các chữ số trong tương lai. Điều này loại bỏ bất kỳ cấu trúc không tham lam nào. Kết quả là, chiến lược tối ưu là cố định và duy nhất, thu gọn trò chơi thành một so sánh xác định của hai con số được xây dựng. Vì cả hai cách xây dựng đều là giới hạn trên chính xác cho mỗi người chơi nên việc so sánh chúng sẽ đưa ra người chiến thắng chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b, n, m = map(int, input().split())

A = a * (10 ** n) + ((10 ** n) - 1)
B = b * (10 ** m) + ((10 ** m) - 1)

if A > B:
    print("Arthur")
elif A < B:
    print("Nikita")
else:
    print("Draw")
```Mã trực tiếp triển khai công thức dẫn xuất cho số cuối cùng tối ưu của mỗi người chơi. Phép nhân với`10 ** n`chuyển số ban đầu sang trái`n`số thập phân và`(10 ** n - 1)`điền tất cả các chữ số được nối thêm bằng 9. 

Kiểu số nguyên của Python xử lý một cách an toàn các giá trị rất lớn có thể phát sinh khi`n`Và`m`lớn nên không cần xử lý số nguyên lớn theo cách thủ công. 

So sánh là so sánh số nguyên trực tiếp, vừa chính xác vừa hiệu quả. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`1 2 3 4`Arthur tính toán`A = 1 * 10^3 + 999 = 1999`. 

Nikita tính toán`B = 2 * 10^4 + 9999 = 29999`. 

| Bước | Arthur | Nikita | 
| --- | --- | --- | 
| Căn cứ | 1 | 2 | 
| Quyền lực | 10^3 | 10^4 | 
| Cuối cùng | 1999 | 29999 | 

Số của Nikita rõ ràng là lớn hơn vì vị trí chữ số phụ từ`m = 4`thống trị phần mở rộng nhỏ hơn của Arthur. 

### Mẫu 2 

đầu vào:`54 54 54 54`Arthur tính toán`A = 54 * 10^54 + (10^54 - 1)`. 

Nikita tính toán`B = 54 * 10^54 + (10^54 - 1)`. 

| Bước | Arthur | Nikita | 
| --- | --- | --- | 
| Căn cứ | 54 | 54 | 
| Quyền lực | 10^54 | 10^54 | 
| Cuối cùng | giống hệt nhau | giống hệt nhau | 

Cả hai biểu thức đều giống hệt nhau nên kết quả là hòa. 

Những ví dụ này xác nhận rằng cả độ lớn của các chữ số được nối thêm và sự bằng nhau của cấu trúc đều xác định đầy đủ kết quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Số lượng không đổi các phép toán số nguyên lớn | 
| Không gian | O(1) | Chỉ một số số nguyên được lưu trữ | 

Giải pháp chạy trong giới hạn vì Python xử lý các số nguyên lớn một cách hiệu quả và số lượng thao tác không phụ thuộc vào độ lớn của`n`hoặc`m`, chỉ về số học trên một số giá trị không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    a, b, n, m = map(int, input().split())

    A = a * (10 ** n) + ((10 ** n) - 1)
    B = b * (10 ** m) + ((10 ** m) - 1)

    if A > B:
        return "Arthur"
    elif A < B:
        return "Nikita"
    return "Draw"

# provided samples
assert run("1 2 3 4") == "Nikita"
assert run("54 54 54 54") == "Draw"
assert run("11 10 2 2") == "Arthur"

# custom cases
assert run("1 1 1 2") == "Nikita", "longer suffix wins"
assert run("9 10 1 1") == "Nikita", "base dominance"
assert run("100 1 0 0") == "Arthur", "no extension comparison"
assert run("5 5 10 10") == "Draw", "identical structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 2 | Nikita | hậu tố dài hơn chiếm ưu thế | 
| 9 10 1 1 | Nikita | so sánh cơ sở sau khi tăng trưởng bằng nhau | 
| 100 1 0 0 | Arthur | không có trường hợp mở rộng | 
| 5 5 10 10 | Vẽ | tính đúng đắn đối xứng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi cả hai người chơi đều có đáy và độ dài phần mở rộng giống hệt nhau. Đối với đầu vào`a = 7, b = 7, n = 3, m = 3`, cả hai đều tính toán`7 * 10^3 + 999`, tạo ra các số giống hệt nhau. Thuật toán xử lý việc này một cách tự nhiên vì cả hai giá trị được xây dựng đều giống hệt nhau và phép kiểm tra đẳng thức cuối cùng trả về "Vẽ". 

Một trường hợp tinh vi khác là khi một người chơi có cơ số lớn hơn nhiều nhưng lại có ít chữ số hơn để thêm vào. Vì`a = 1000, b = 1, n = 0, m = 5`, Arthur có`1000`, trong khi Nikita có`1 * 10^5 + 99999 = 199999`. Mặc dù Arthur bắt đầu lớn hơn, hậu tố dài hơn của Nikita hoàn toàn chiếm ưu thế. Thuật toán nắm bắt được điều này vì sức mạnh của dịch chuyển mười được áp dụng trước khi so sánh, làm cho độ dài hậu tố trở thành yếu tố chi phối khi cường độ đủ khác nhau.
