---
title: "CF 102777A - \u041c\u0430\u043a\u0441\u0438\u043c\u0430\u043b\u044c\u043d\u044b\u0439 \u0434\u0435\u043b\u0438\u0442\u0435\u043b\u044c"
description: "Đầu vào là một số nguyên dương được viết dưới dạng chuỗi nhị phân. Nhiệm vụ là tìm số mũ b lớn nhất sao cho số được biểu thị có thể chia cho 2^b mà không có số dư. Trong hệ nhị phân, chia cho lũy thừa hai có ý nghĩa rất trực tiếp."
date: "2026-07-27T20:19:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102777
codeforces_index: "A"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 19), \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0426\u0435\u043d\u0442\u0440\u0430\u043b\u044c\u043d\u043e\u0439 \u0420\u043e\u0441\u0441\u0438\u0438, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434"
rating: 0
weight: 102777
solve_time_s: 37
verified: true
draft: false
---

[CF 102777A - \u041c\u0430\u043a\u0441\u0438\u043c\u0430\u043b\u044c\u043d\u044b\u0439 \u0434\u0435\u043b\u0438\u0442\u0435\u043b\u044c](https://codeforces.com/problemset/problem/102777/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào là một số nguyên dương được viết dưới dạng chuỗi nhị phân. Nhiệm vụ là tìm số mũ lớn nhất`b`sao cho số được biểu diễn có thể chia cho`2^b`không có phần dư. 

Trong hệ nhị phân, chia cho lũy thừa hai có ý nghĩa rất trực tiếp. Loại bỏ một thừa số của hai sẽ dịch biểu diễn nhị phân sang phải một vị trí. Một số nhị phân chia hết cho`2`,`4`,`8`, v.v. chính xác khi nó có đủ số 0 bit ở cuối. Câu trả lời là số số 0 liên tiếp ở cuối chuỗi nhị phân. 

Số chữ số trong đầu vào có thể lên tới 1000. Con số này đủ nhỏ để chúng ta có thể kiểm tra trực tiếp toàn bộ chuỗi, nhưng điều đó cũng có nghĩa là chúng ta không nên dựa vào việc chuyển đổi nó thành kiểu số nguyên thông thường. Số 1000 bit vượt xa phạm vi số nguyên có chiều rộng cố định tích hợp trong hầu hết các ngôn ngữ. Quét tuyến tính dễ dàng đủ nhanh vì nó chỉ thực hiện khoảng một nghìn kiểm tra ký tự. 

Các trường hợp cạnh chính đến từ vị trí cuối cùng`1`trong biểu diễn nhị phân. Nếu số đó đã là số lẻ thì không có thừa số nào là hai. Ví dụ, đầu vào`1011`đại diện cho 11, đầu ra là`0`. Một giải pháp bất cẩn đếm số 0 từ phía sai có thể trả về giá trị dương không chính xác. 

Một trường hợp biên khác là lũy thừa của chính nó. Đối với đầu vào`1000`, câu trả lời là`3`bởi vì`1000₂ = 8 = 2^3`. Một triển khai đếm số ký tự sau ký tự đầu tiên`1`thay vì số lượng số 0 ở cuối sẽ không thành công trong những trường hợp như vậy. 

Số hợp lệ nhỏ nhất là`1`. Dạng nhị phân của nó không có số 0 ở cuối, vì vậy kết quả đúng là`0`. Mã giả định luôn có ít nhất một số 0 sau số cuối cùng`1`có thể truy cập vào một vị trí không hợp lệ hoặc tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là liên tục chia số đó cho hai trong khi nó vẫn chia hết cho hai. Sau mỗi lần chia thành công, câu trả lời sẽ tăng thêm một. Điều này đúng về mặt toán học vì số phép chia thành công chính xác là lũy thừa cao nhất của hai số chia cho giá trị ban đầu. 

Vấn đề là số được đưa ra dưới dạng một chuỗi nhị phân có tối đa 1000 chữ số, do đó việc thực hiện phép chia lặp lại đòi hỏi các phép toán chuỗi đắt tiền hoặc số học nhị phân thủ công. Mỗi phép chia có thể chạm vào nhiều chữ số và trong trường hợp xấu nhất là một số như`1000...000`chứa 999 thừa số của 2. Một mô phỏng có thể thực hiện gần`1000 * 1000`các thao tác chữ số, điều này là không cần thiết. 

Quan sát quan trọng là việc chia cho hai trong hệ nhị phân chỉ cần loại bỏ bit cuối cùng. Điều duy nhất quan trọng là có bao nhiêu bit 0 xuất hiện trước bit cuối cùng khi đọc từ cuối chuỗi. Thay vì thay đổi số nhiều lần, chúng ta có thể trực tiếp kiểm tra cách biểu diễn của nó. 

Phương pháp brute-force hoạt động vì mọi phép chia đều loại bỏ một thừa số của hai, nhưng nó không thành công vì nó lặp lại công việc mà biểu diễn nhị phân đã bộc lộ. Số 0 ở cuối cung cấp thông tin tương tự trong một lần truyền. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) trong trường hợp xấu nhất | O(n) | Quá chậm so với quan sát trực tiếp | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc biểu diễn nhị phân dưới dạng chuỗi. Giữ nó dưới dạng văn bản sẽ tránh mọi vấn đề với kích thước số nguyên vì đầu vào có thể chứa tới 1000 bit. 
2. Bắt đầu từ ký tự cuối cùng và di chuyển sang trái trong khi ký tự hiện tại`0`. Mỗi số 0 được tìm thấy đại diện cho một thừa số của hai, bởi vì mỗi số 0 ở cuối có nghĩa là số đó chia hết cho một lũy thừa khác của hai. 
3. Dừng lại khi đến đích đầu tiên`1`. Số số 0 gặp phải là số mũ tối đa`b`. 
4. In bộ đếm. 

Lý do điều này có tác dụng là vì mọi số nhị phân dương đều có một dạng duy nhất trong đó một số tiền tố kết thúc bằng một`1`và tất cả các bit còn lại là số 0. Những số 0 cuối cùng đó chính xác là thừa số của hai có thể loại bỏ được. Một lần`1`đạt đến số còn lại là số lẻ nên không thể chia cho hai được nữa. 

## Tại sao nó hoạt động 

Thuật toán duy trì sự bất biến rằng các ký tự được đếm chính xác là các thừa số bị loại bỏ của hai. Số nhị phân tận cùng bằng một số 0 thì chia hết cho hai. Nếu nó kết thúc bằng hai số 0 thì nó chia hết cho 4, v.v. Quá trình quét đếm mọi số 0 như vậy và dừng ở bit đầu tiên để ngăn cản phép chia khác. Vì số nhị phân lẻ luôn kết thúc bằng`1`, điểm dừng chứng tỏ rằng không có lũy thừa nào lớn hơn của 2 có thể chia số ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()

    ans = 0
    i = len(s) - 1

    while i >= 0 and s[i] == '0':
        ans += 1
        i -= 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp giữ đầu vào ở dạng chuỗi vì việc chuyển đổi giá trị nhị phân 1000 bit thành số nguyên là không cần thiết và có thể vượt quá giới hạn số nguyên bình thường trong nhiều môi trường. 

Biến`i`bắt đầu ở bit cuối cùng vì khả năng chia hết cho lũy thừa hai chỉ phụ thuộc vào các bit thấp. Vòng lặp tăng câu trả lời chính xác khi nó tìm thấy số 0 ở cuối, sau đó di chuyển về phía các bit quan trọng hơn. 

Kiểm tra điều kiện vòng lặp`i >= 0`trước khi truy cập`s[i]`, ngăn chặn một chỉ mục không hợp lệ. Đối với số nhị phân dương hợp lệ, quá trình quét cuối cùng sẽ đạt đến`1`, nhưng việc kiểm tra ranh giới cũng làm cho mã trở nên an toàn. 

Không có phép toán số học nào trên số thực được thực hiện, do đó không có vấn đề tràn và việc triển khai tuân theo thuộc tính nhị phân được sử dụng trong thuật toán. 

## Ví dụ đã hoạt động 

Hãy xem xét số nhị phân`1011`. 

| Vị trí hiện tại | Bit hiện tại | Trả lời | 
| --- | --- | --- | 
| 3 | 1 | 0 | 

Quá trình quét bắt đầu ở cuối. Bit cuối cùng đã có rồi`1`, do đó không có số 0 ở cuối. Số này là số lẻ, nghĩa là nó không chia hết cho hai. 

Hãy xem xét số nhị phân`101000`. 

| Vị trí hiện tại | Bit hiện tại | Trả lời | 
| --- | --- | --- | 
| 5 | 0 | 1 | 
| 4 | 0 | 2 | 
| 3 | 0 | 3 | 
| 2 | 1 | 3 | 

Quá trình quét đếm ba số 0 ở cuối và dừng ở số đầu tiên`1`. Câu trả lời là`3`, nghĩa là số đó chia hết cho`2^3`nhưng không phải bởi`2^4`. 

Những ví dụ này cho thấy hai tình huống quan trọng: một số không có thừa số 2 và một số có nhiều thừa số bằng 2 được biểu thị trực tiếp bằng các bit ở cuối của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi bit của chuỗi nhị phân được kiểm tra nhiều nhất một lần. | 
| Không gian | O(1) | Chỉ có một bộ đếm và một chỉ mục được lưu trữ. | 

Độ dài đầu vào tối đa chỉ là 1000 ký tự, do đó quét tuyến tính dễ dàng phù hợp với giới hạn nhất định. Thuật toán cũng chia tỷ lệ một cách tự nhiên thành các chuỗi nhị phân lớn hơn nhiều vì nó không bao giờ thực hiện số học trên giá trị được biểu thị. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(data: str) -> str:
    s = data.strip()

    ans = 0
    i = len(s) - 1

    while i >= 0 and s[i] == '0':
        ans += 1
        i -= 1

    return str(ans)

def run(inp: str) -> str:
    return solve(inp)

assert run("1\n") == "0", "minimum value"
assert run("1000\n") == "3", "power of two"
assert run("1011\n") == "0", "odd number"
assert run("11110000\n") == "4", "multiple trailing zeroes"
assert run("1000000000\n") == "9", "large power of two"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`0`| Xử lý giá trị dương nhỏ nhất có thể. | 
|`1000`|`3`| Xác nhận lũy thừa của hai được tính chính xác. | 
|`1011`|`0`| Kiểm tra số lẻ không có thừa số hai. | 
|`11110000`|`4`| Kiểm tra việc đếm một số số 0 ở cuối. | 
|`1000000000`|`9`| Kiểm tra một chuỗi dài các số 0 và đếm ranh giới. | 

## Vỏ cạnh 

Đối với đầu vào`1011`, thuật toán bắt đầu ở ký tự cuối cùng, xem`1`, và dừng lại ngay lập tức. Câu trả lời vẫn còn`0`. Điều này đúng vì giá trị được biểu thị là số lẻ và không thể chia cho hai. 

Đối với đầu vào`1000`, quá trình quét sẽ truy cập ba số 0 trước khi đến số đầu tiên`1`. Bộ đếm trở thành`3`, phù hợp với thực tế rằng`1000₂`là`8`, Và`8`chia hết cho`2^3`. 

Đối với đầu vào`1`, chỉ mục bắt đầu ở ký tự duy nhất. Vì nó là`1`, vòng lặp không bao giờ thực thi và đầu ra là`0`. Điều này tránh việc giả định sự tồn tại của các số 0 ở cuối. 

Đối với đầu vào`1000000000`, quá trình quét đếm mọi số 0 sau số đầu tiên`1`. Có chín số 0 như vậy nên đáp án là`9`. Thuật toán không nhầm lẫn độ dài của chuỗi với số mũ vì nó chỉ tính hậu tố bao gồm các bit 0.
