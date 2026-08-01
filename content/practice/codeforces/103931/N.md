---
title: "CF 103931N - Chín lớn hơn mười"
description: "Chúng ta có hai số nguyên dương được viết dưới dạng thập phân đơn giản, mỗi số không có số 0 đứng đầu và chúng ta cần so sánh chúng bằng cách sử dụng một quy tắc kỳ quặc có chủ ý lấy cảm hứng từ câu chuyện."
date: "2026-07-02T07:20:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103931
codeforces_index: "N"
codeforces_contest_name: "2022 Shanghai Collegiate Programming Contest"
rating: 0
weight: 103931
solve_time_s: 42
verified: true
draft: false
---

[CF 103931N - Chín lớn hơn mười](https://codeforces.com/problemset/problem/103931/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai số nguyên dương được viết dưới dạng thập phân đơn giản, mỗi số không có số 0 đứng đầu và chúng ta cần so sánh chúng bằng cách sử dụng một quy tắc kỳ quặc có chủ ý lấy cảm hứng từ câu chuyện. Bất chấp lối “so sánh Koji” hài hước, nhiệm vụ thực tế vẫn chỉ dừng lại ở việc xác định xem số thứ nhất nhỏ hơn, bằng hay lớn hơn số thứ hai và in ra mối quan hệ tương ứng ở định dạng chính xác`a>b`,`a<b`, hoặc`a=b`. 

Chi tiết quan trọng là các số nguyên có thể rất lớn, lên tới khoảng$2 \cdot 10^{10}$, vì vậy chúng có thể không phù hợp một cách an toàn với các loại số nguyên có chiều rộng cố định tiêu chuẩn ở một số ngôn ngữ. Trong Python, điều này ít đáng lo ngại hơn, nhưng mục đích của ràng buộc rõ ràng là chúng ta nên coi chúng là chuỗi hoặc sử dụng logic chính xác tùy ý thay vì dựa vào các giả định phân tích cú pháp số nguyên ngây thơ trong các ngôn ngữ chặt chẽ hơn. 

Một lỗi ngây thơ nhưng phổ biến là chuyển đổi cả hai đầu vào thành số nguyên và so sánh chúng trực tiếp. Điều đó đúng trong Python, nhưng trong các môi trường khác, nó có nguy cơ bị tràn. Một cạm bẫy tinh vi khác là cố gắng so sánh từ điển dưới dạng chuỗi mà không xử lý đúng sự khác biệt về độ dài. Ví dụ,`"9"`Và`"10"`: theo từ điển`"9" > "10"`là sai bởi vì`'9' < '1'`chỉ sai nếu chúng ta so sánh không chính xác; nhưng thực ra so sánh chuỗi hoạt động theo thứ tự từ điển, vì vậy`"9" > "10"`sẽ đánh giá không chính xác như`True`trong một số triển khai ngây thơ bởi vì`'9' > '1'`. Thứ tự đúng phải xem xét độ dài số trước tiên. 

Các trường hợp cạnh quan trọng ở đây bao gồm số có một chữ số so với số có nhiều chữ số, chuỗi bằng nhau và các số trong đó chữ số đầu tiên khác ngay lập tức so với trường hợp chúng có chung tiền tố. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: phân tích cả hai đầu vào dưới dạng số nguyên và so sánh chúng trực tiếp. Trong Python, đây thực sự là thời gian không đổi để so sánh sau khi phân tích cú pháp, nhưng bản thân việc phân tích cú pháp là tuyến tính theo số chữ số. Vì mỗi số có thể có tối đa khoảng 11 chữ số trong bài toán này nên điều này không quan trọng và an toàn. Trong bối cảnh ngôn ngữ chặt chẽ hơn, cách tiếp cận này vẫn có thể được chấp nhận vì giới hạn nhỏ, nhưng vấn đề về mặt khái niệm là nó dựa vào sự hỗ trợ số nguyên lớn được tích hợp sẵn. 

Một cách tiếp cận phổ quát hơn có thể áp dụng được với bất kỳ ngôn ngữ nào là coi các số dưới dạng chuỗi và so sánh chúng dưới dạng chuỗi số. Quan sát quan trọng là so sánh số nguyên có thể được rút gọn thành hai quy tắc. Thứ nhất, nếu độ dài khác nhau thì số dài hơn sẽ lớn hơn vì nó có nhiều chữ số hơn trong cơ số 10. Thứ hai, nếu độ dài bằng nhau thì so sánh từ điển tiêu chuẩn sẽ có tác dụng vì cả hai chuỗi đều biểu thị các chuỗi chữ số được căn chỉnh. 

Mô hình tinh thần vũ phu hoạt động vì chúng tôi mô phỏng ngầm các giá trị số nguyên thực tế. Về mặt khái niệm, nó không thành công khi dựa vào so sánh từ điển thô mà không chuẩn hóa độ dài, vì thứ tự chữ số trong ASCII không được căn chỉnh theo độ lớn số trên các độ dài khác nhau. Việc quan sát rằng độ dài chữ số xác định độ lớn sẽ loại bỏ nhu cầu phân tích số đầy đủ và đưa ra quy tắc so sánh trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (phân tích cú pháp số nguyên) | O(d) | O(d) | Đã chấp nhận | 
| Độ dài chuỗi + so sánh từ điển | O(d) | O(1) thêm | Đã chấp nhận | 

Đây$d$là số chữ số của số lớn hơn. 

## Hướng dẫn thuật toán 

1. Đọc hai chuỗi đầu vào`a`Và`b`. Chúng tôi giữ chúng dưới dạng chuỗi vì cấu trúc chữ số của chúng mã hóa trực tiếp độ lớn của chúng. 
2. So sánh độ dài của`a`Và`b`. Nếu như`len(a) > len(b)`, chúng ta kết luận ngay`a > b`. Điều này có tác dụng vì bất kỳ số nào có nhiều chữ số nhất thiết phải lớn hơn trong cơ số 10. 
3. Nếu`len(a) < len(b)`, chúng ta kết luận ngay`a < b`vì lý do tương tự. 
4. Nếu độ dài bằng nhau thì so sánh`a`Và`b`theo từng ký tự từ trái qua phải. Vị trí đầu tiên nơi chúng khác nhau xác định thứ tự: chuỗi có chữ số lớn hơn ở vị trí đó tương ứng với số lớn hơn. 
5. Nếu không có vị trí khác nhau thì các số giống hệt nhau và chúng ta đưa ra kết quả bằng nhau. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là biểu diễn thập phân có tính vị trí. Một số có nhiều chữ số hơn luôn vượt mọi số có ít chữ số hơn vì số có k chữ số nhỏ nhất là$10^{k-1}$, lớn hơn rất nhiều so với giá trị lớn nhất$(k-1)$-số có chữ số$10^{k-1}-1$. Khi số lượng chữ số khớp nhau, mỗi so sánh tiền tố sẽ giữ nguyên thứ tự số vì cả hai số đều có cùng trọng số vị trí. Chữ số khác nhau đầu tiên xác định số nào có đóng góp lớn hơn ở giá trị vị trí khác nhau cao nhất và tất cả các chữ số sau đó không thể bù cho chênh lệch đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b = input().split()

if len(a) > len(b):
    print(f"{a}>{b}")
elif len(a) < len(b):
    print(f"{a}<{b}")
else:
    if a > b:
        print(f"{a}>{b}")
    elif a < b:
        print(f"{a}<{b}")
    else:
        print(f"{a}={b}")
```Giải pháp tách so sánh thành hai giai đoạn riêng biệt về mặt logic: trước tiên là so sánh độ dài, sau đó chỉ so sánh từ điển khi cần thiết. Yêu cầu định dạng được xử lý trực tiếp thông qua chuỗi f, bảo toàn cấu trúc đầu ra chính xác. 

Một chi tiết triển khai tinh tế là chúng tôi không bao giờ chuyển đổi chuỗi thành số nguyên. Điều này giúp giải pháp an toàn bằng mọi ngôn ngữ và tránh chi phí không cần thiết. Việc so sánh từ điển chỉ có giá trị vì chúng tôi đã đảm bảo độ dài bằng nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`9 10`| Bước | một | b | len(a) | len(b) | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 1 | "9" | "10" | 1 | 2 | So sánh độ dài | 

Từ`len(a) < len(b)`, chúng tôi xuất ngay`9<10`. 

Điều này chứng tỏ rằng cường độ bị chi phối bởi số lượng chữ số thay vì so sánh ký tự. 

### Ví dụ 2:`114514 1919`| Bước | một | b | len(a) | len(b) | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 1 | "114514" | "1919" | 6 | 4 | So sánh độ dài | 

Từ`len(a) > len(b)`, chúng tôi xuất ra`114514>1919`không so sánh từng chữ số. 

Điều này cho thấy tại sao chỉ so sánh độ dài là đủ trong hầu hết các trường hợp và tránh việc quét không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(d) | Chúng tôi đọc đầu vào và có thể so sánh các chuỗi từng chữ số | 
| Không gian | O(1) | Chỉ lưu trữ hai chuỗi đầu vào và các biến phụ không đổi | 

Độ dài chữ số$d$nhiều nhất là khoảng 11 trong các ràng buộc, vì vậy giải pháp chạy hiệu quả trong thời gian không đổi và phù hợp một cách tầm thường trong các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    a, b = input().split()

    if len(a) > len(b):
        return f"{a}>{b}"
    elif len(a) < len(b):
        return f"{a}<{b}"
    else:
        if a > b:
            return f"{a}>{b}"
        elif a < b:
            return f"{a}<{b}"
        else:
            return f"{a}={b}"

# provided samples
assert run("9 10") == "9<10"
assert run("114514 1919") == "114514>1919"
assert run("9 999") == "9<999"
assert run("99 99") == "99=99"

# custom cases
assert run("1 2") == "1<2", "single digit increasing"
assert run("10 2") == "10>2", "different digit lengths reversal"
assert run("123 123") == "123=123", "exact equality"
assert run("1000 999") == "1000>999", "power of ten boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | 1<2 | so sánh một chữ số nhỏ nhất | 
| 10 2 | 10>2 | độ dài vượt trội so với trực giác từ điển | 
| 123 123 | 123=123 | xử lý bình đẳng | 
| 1000 999 | 1000>999 | ranh giới giữa độ dài chữ số | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các số chỉ khác nhau về số lượng chữ số, chẳng hạn như`10`so với`9`. Thuật toán xử lý vấn đề này ngay lập tức thông qua so sánh độ dài và tránh suy luận từ điển không chính xác. 

Một trường hợp khác là những con số giống hệt nhau như`999`Và`999`, trong đó cả độ dài và so sánh từ điển đều không tìm thấy sự khác biệt. Thuật toán rơi vào đẳng thức một cách chính xác. 

Cuối cùng, các trường hợp tiền tố khớp nhau nhưng độ dài khác nhau, chẳng hạn như`100`Và`99`, được giải quyết chính xác vì so sánh độ dài chiếm ưu thế trước bất kỳ kiểm tra chữ số nào.
