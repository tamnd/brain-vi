---
title: "CF 352A - Jeff và chữ số"
description: "Chúng ta được cấp một bộ thẻ, mỗi thẻ hiển thị chữ số 0 hoặc chữ số 5. ​​Từ những thẻ này, chúng ta có thể chọn bất kỳ tập hợp con nào và sắp xếp các chữ số đã chọn thành một số được viết thành một dòng. Mục tiêu của chúng ta là xây dựng số lớn nhất có thể chia hết cho 90."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 352
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 204 (Div. 2)"
rating: 1000
weight: 352
solve_time_s: 146
verified: true
draft: false
---

[CF 352A - Jeff và Digits](https://codeforces.com/problemset/problem/352/A) 

**Đánh giá:** 1000 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 2 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bộ thẻ, mỗi thẻ hiển thị chữ số 0 hoặc chữ số 5. Từ những thẻ này, chúng ta có thể chọn bất kỳ tập hợp con nào và sắp xếp các chữ số đã chọn thành một số được viết thành một dòng. Mục tiêu của chúng ta là xây dựng số lớn nhất có thể chia hết cho 90. 

Một số chia hết cho 90 phải thỏa mãn đồng thời hai điều kiện. Đầu tiên, nó phải chia hết cho 10, điều này buộc chữ số cuối cùng của nó là 0. Thứ hai, nó phải chia hết cho 9, nghĩa là tổng các chữ số của nó phải chia hết cho 9. Vì chúng ta chỉ có các chữ số 0 và 5 nên những ràng buộc này hạn chế mạnh mẽ những gì có thể. 

Đầu ra phải là số tối đa như vậy theo độ lớn từ điển, đối với các số không có số 0 đứng đầu tương đương với việc tối đa hóa số chữ số và đặt các chữ số lớn hơn trước. Nếu không tạo được số hợp lệ, chúng ta xuất -1. Có một cách giải thích đặc biệt: số 0 được cho phép và được coi là hợp lệ nếu không có số 0 đứng đầu. 

Các ràng buộc nhỏ, tối đa là 1000 thẻ, vì vậy việc đếm tuyến tính và các quyết định số học đơn giản là đủ. Bất cứ điều gì liên quan đến hoán vị hoặc liệt kê tập hợp con sẽ không cần thiết. 

Các trường hợp cạnh chính phát sinh từ cách các ràng buộc chia hết tương tác với các loại chữ số giới hạn. Một sai lầm ngây thơ là cố gắng sử dụng tất cả các số 0 và số 5 hoặc chỉ kiểm tra khả năng chia hết cho 10 mà không đảm bảo chia hết cho 9. Một lỗi phổ biến khác là quên rằng một số hợp lệ phải kết thúc bằng 0, nghĩa là phải tồn tại ít nhất một số 0, ngoại trừ trường hợp suy biến khi câu trả lời chính xác là "0". 

Ví dụ: nếu chúng ta chỉ có số 5 và không có số 0, chẳng hạn như đầu vào`[5, 5, 5]`, chúng ta không thể tạo ra bất kỳ số nào chia hết cho 10, vì vậy đáp án là`-1`. Nếu chúng ta có số 0 nhưng có quá ít số năm để làm cho tổng chia hết cho 9, chẳng hạn như một 0 và một năm, thì ứng cử viên duy nhất là "0". 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các tập hợp con của thẻ và tất cả các hoán vị của từng tập hợp con để tạo thành số, sau đó kiểm tra khả năng chia hết cho 90 và giữ mức tối đa. Điều này đúng vì nó khám phá mọi khả năng, nhưng nó không khả thi. Với tối đa 1000 thẻ, số lượng tập hợp con là$2^{1000}$và thậm chí bỏ qua các tập hợp con, việc hoán vị các phần tử đã chọn sẽ dẫn đến tăng trưởng giai thừa. Cách tiếp cận này sụp đổ ngay lập tức dưới những ràng buộc. 

Nhận xét quan trọng là cấu trúc của vấn đề loại bỏ gần như toàn bộ quyền tự do. Vì chỉ tồn tại các chữ số 0 và 5 nên mọi số được xây dựng đều được xác định hoàn toàn bằng số lượng 5 và bao nhiêu số 0 mà chúng ta đưa vào. Chia hết cho 10 lực ít nhất một số không. Số chia hết cho 9 chỉ phụ thuộc vào tổng các chữ số$5 \cdot k$Ở đâu$k$là số lượng người được chọn. Vì vậy chúng ta cần$5k \equiv 0 \pmod{9}$, tương đương với$k \equiv 0 \pmod{9}$vì 5 và 9 là số nguyên tố cùng nhau. 

Vì vậy, giải pháp trở thành: chọn số lượng năm lớn nhất có thể chia hết cho 9 và sử dụng tất cả các số 0 có sẵn. Nếu không có số 5 nào như vậy tồn tại thì câu trả lời duy nhất có thể là 0 (nếu có ít nhất một số 0 tồn tại) hoặc không thể. 

Sau đó, chúng tôi xây dựng số bằng cách in ra nhiều số năm theo sau là tất cả các số 0, điều này đảm bảo giá trị tối đa vì việc đặt các chữ số lớn hơn trước sẽ tối đa hóa số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu số 0 và bao nhiêu số 5 trong đầu vào. Điều này làm giảm vấn đề về lý luận về tần số hơn là về từng thẻ riêng lẻ. 
2. Kiểm tra xem chúng ta có ít nhất một số 0 hay không. Nếu không, chúng ta không thể tạo ra số nào chia hết cho 10 nên đáp án ngay là -1. 
3. Xác định xem có thể sử dụng bao nhiêu số năm sao cho tổng số của chúng chia hết cho 9. Chúng ta lấy bội số lớn nhất của 9 nhỏ hơn hoặc bằng tổng số số năm. Điều này tối đa hóa số chữ số đầu trong khi vẫn giữ nguyên khả năng chia hết cho 9. 
4. Nếu số năm được tính toán này bằng 0, chúng ta không thể xây dựng một số chứa 5s thỏa mãn khả năng chia hết cho 9. Trong trường hợp này, điều tốt nhất chúng ta có thể làm là xuất ra một số 0 duy nhất. 
5. Mặt khác, xây dựng kết quả bằng cách in ra nhiều số 5 theo sau là tất cả các số 0. Thứ tự này đảm bảo số càng lớn càng tốt vì tất cả các chữ số có nghĩa là 5 và tất cả các chữ số ở cuối là 0. 

### Tại sao nó hoạt động 

Mọi số hợp lệ đều phải kết thúc bằng 0, vì vậy bắt buộc phải có ít nhất một số 0. Khi ràng buộc đó được thỏa mãn, yêu cầu còn lại là chia hết cho 9, chỉ phụ thuộc vào tổng các chữ số. Vì các số 0 không ảnh hưởng đến tổng nên quyết định có ý nghĩa duy nhất là bao gồm bao nhiêu số năm. Tối đa hóa số lượng năm trong khi giữ cho số lượng của chúng chia hết cho 9 sẽ tối đa hóa cả độ dài và giá trị từ điển của số. Không có sự sắp xếp lại nào có thể cải thiện kết quả vì tất cả các hoán vị hợp lệ với cùng nhiều tập hợp đều tạo ra thứ tự tối ưu giống nhau khi sắp xếp giảm dần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    
    cnt0 = arr.count(0)
    cnt5 = arr.count(5)
    
    if cnt0 == 0:
        print(-1)
        return
    
    use5 = (cnt5 // 9) * 9
    
    if use5 == 0:
        print(0)
        return
    
    print("5" * use5 + "0" * cnt0)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đếm số không và số năm trong một lần chuyển. Sau đó, nó thực thi yêu cầu chia hết cho 10 bằng cách kiểm tra xem có tồn tại ít nhất một số 0 hay không. Sau đó, nó tính toán số lượng năm có thể sử dụng bằng cách làm tròn xuống bội số gần nhất của 9, trực tiếp thực thi tính chia hết cho 9. Cấu trúc cuối cùng sẽ in tất cả các số năm đã chọn trước, sau đó là tất cả các số 0, đảm bảo giá trị số tối đa. 

Điểm tinh tế là chúng tôi không bao giờ cố gắng xen kẽ các chữ số hoặc kiểm tra hoán vị, bởi vì cấu trúc tối ưu là bắt buộc: tất cả các số năm phải đứng trước số 0 trong bất kỳ cách sắp xếp giá trị tối đa nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
5 0 5 0
```Chúng tôi đếm hai số không và hai số năm. 

| Bước | cnt0 | cnt5 | sử dụng5 | quyết định | 
| --- | --- | --- | --- | --- | 
| ban đầu | 2 | 2 | - | bắt đầu | 
| kiểm tra số không | 2 | 2 | - | hợp lệ | 
| tính toán sử dụng5 | 2 | 2 | 0 | 2 // 9 = 0 | 
| kết quả | - | - | - | đầu ra 0 | 

Vì chúng ta không thể tạo bội số của 9 bằng cách sử dụng số năm nên cấu trúc hợp lệ duy nhất là một số 0 duy nhất. 

### Ví dụ 2 

đầu vào:```
10
5 5 5 5 5 5 5 5 5 0
```Chúng ta có chín số năm và một số không. 

| Bước | cnt0 | cnt5 | sử dụng5 | quyết định | 
| --- | --- | --- | --- | --- | 
| ban đầu | 1 | 9 | - | bắt đầu | 
| kiểm tra số không | 1 | 9 | - | hợp lệ | 
| tính toán sử dụng5 | 1 | 9 | 9 | 9 // 9 = 9 | 
| kết quả | - | - | 9 | 5555555550 | 

Chúng ta sử dụng tất cả số 5 vì 9 chia hết cho 9, sau đó thêm số 0 vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | đếm các chữ số trong một lần | 
| Không gian | O(1) | chỉ có bộ đếm tần số được lưu trữ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì n tối đa là 1000 và tất cả các phép toán đều là số học theo thời gian không đổi cộng với cấu trúc chuỗi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    arr = list(map(int, input().split()))
    cnt0 = arr.count(0)
    cnt5 = arr.count(5)

    if cnt0 == 0:
        return "-1"
    use5 = (cnt5 // 9) * 9
    if use5 == 0:
        return "0"
    return "5" * use5 + "0" * cnt0

# provided sample
assert run("4\n5 0 5 0\n") == "0"

# all zeros minimum case
assert run("1\n0\n") == "0"

# no zeros impossible
assert run("3\n5 5 5\n") == "-1"

# enough fives for multiple of 9
assert run("10\n" + "5 "*9 + "0\n") == "5555555550"

# extra fives beyond multiple of 9
assert run("11\n" + "5 "*10 + "0\n") == "5555555550"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 0 | 0 | xây dựng hợp lệ tối thiểu | 
| chỉ 5s | -1 | không thể không có số không | 
| 9 năm + không | 5555555550 | ranh giới chia hết chính xác | 
| 10 năm + không | 5555555550 | hành vi làm tròn xuống | 

## Vỏ cạnh 

Một trường hợp cạnh tới hạn là khi số 0 tồn tại nhưng số 5 không thể tạo thành bội số của 9. Đối với đầu vào`0 5`, chúng ta có một số không và một năm. Thuật toán đặt số năm có thể sử dụng được về 0 và chỉ xuất ra "0". Điều này đúng vì bất kỳ sự bao gồm nào của năm đều phá vỡ tính chia hết cho 9. 

Một trường hợp khác là khi không có số 0 nào cả, chẳng hạn`5 5 5 5 5`. Mặc dù chúng ta có thể tạo ra những số lớn từ số 5 nhưng không có số nào tận cùng bằng 0, do đó việc chia hết cho 10 sẽ thất bại ngay lập tức. Thuật toán trả về chính xác -1 trước khi thử bất kỳ lý do nào khác.
