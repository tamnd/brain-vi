---
title: "CF 105223I - Fofo Yêu Bitset"
description: "Chúng ta được cung cấp một số chuỗi độc lập và đối với mỗi chuỗi, chúng ta phải quyết định xem nó có chứa một mẫu cụ thể hay không, từ “bitset”, như một khối liền kề bên trong nó."
date: "2026-06-24T16:41:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105223
codeforces_index: "I"
codeforces_contest_name: "HIAST Collegiate Programming Contest 2024"
rating: 0
weight: 105223
solve_time_s: 41
verified: true
draft: false
---

[CF 105223I - Fofo thích Bitset](https://codeforces.com/problemset/problem/105223/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số chuỗi độc lập và đối với mỗi chuỗi, chúng ta phải quyết định xem nó có chứa một mẫu cụ thể hay không, từ “bitset”, như một khối liền kề bên trong nó. Nếu mẫu đó xuất hiện ở bất kỳ đâu trong chuỗi mà không sắp xếp lại các ký tự, chúng tôi sẽ đưa ra một phản hồi khẳng định cố định. Nếu không, chúng tôi sẽ đưa ra một phản hồi tiêu cực cố định. 

Mỗi chuỗi đầu vào đều ngắn, có độ dài tối đa 26 ký tự và có tối đa 100 chuỗi như vậy. Điều này ngay lập tức cho chúng ta biết rằng ngay cả việc quét đơn giản mọi vị trí trong mỗi chuỗi cũng cực kỳ rẻ. Kiểm tra từng ký tự đầy đủ cho mỗi trường hợp thử nghiệm tối đa là vài chục thao tác, do đó, ngay cả cách tiếp cận đơn giản nhất cũng nằm trong giới hạn. 

Không có ràng buộc ẩn nào như bảng chữ cái lớn, cập nhật động hoặc nhiều truy vấn trên mỗi chuỗi. Nguồn sai sót duy nhất có thể xảy ra là hiểu nhầm ý nghĩa của “chuỗi con” trong ngữ cảnh này hoặc xử lý sai một phần các kết quả khớp trùng lặp không chính xác. 

Trường hợp cạnh tinh tế xuất hiện khi chuỗi chứa thứ gì đó gần với mục tiêu nhưng không khớp chính xác với nó. Ví dụ: “bitnoset” trông giống “bitset” nhưng có một ký tự phụ ở giữa, vì vậy nó sẽ bị từ chối. Một trường hợp khác là những lần xuất hiện lặp đi lặp lại như “obitsetobitseto”, trường hợp này vẫn nên được chấp nhận vì chỉ một lần xuất hiện hợp lệ là đủ. 

## Phương pháp tiếp cận 

Giải pháp brute-force là kiểm tra mọi vị trí bắt đầu có thể có trong chuỗi và cố gắng khớp từng ký tự “bitset” của từ mục tiêu. Đối với mỗi chỉ mục i, chúng tôi so sánh chuỗi con bắt đầu từ i với độ dài 6 với mẫu. Nếu tất cả các ký tự khớp nhau, chúng ta sẽ chấp nhận ngay chuỗi đó. 

Vì độ dài chuỗi nhiều nhất là 26 nên có nhiều nhất 26 vị trí bắt đầu. Đối với mỗi lần bắt đầu, chúng tôi so sánh tối đa 6 ký tự. Điều đó mang lại chi phí trong trường hợp xấu nhất là khoảng 156 so sánh ký tự cho mỗi trường hợp thử nghiệm, con số này không đáng kể ngay cả ở 100 trường hợp thử nghiệm. 

Quan sát chính là không cần bất kỳ cấu trúc dữ liệu tiền xử lý hoặc nâng cao nào. Vấn đề hoàn toàn là tìm kiếm chuỗi con mẫu cố định với bảng chữ cái nhỏ và kích thước đầu vào nhỏ. Bất kỳ tối ưu hóa nào ngoài việc quét trực tiếp đều không cần thiết. 

Một cách trừu tượng hơn để thấy điều đó là chúng ta đang kiểm tra tư cách thành viên của một chuỗi cố định bên trong một chuỗi khác. Vì độ dài mẫu không đổi và rất nhỏ nên chúng ta có thể coi nó như một phép kiểm tra liên tục theo thời gian cho mỗi vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét Brute Force trên tất cả các vị trí | O(n · 6) | O(1) | Đã chấp nhận | 
| Tìm kiếm chuỗi con trực tiếp (cùng ý tưởng) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Mỗi trường hợp thử nghiệm được xử lý độc lập vì không có trạng thái chia sẻ giữa các chuỗi. 
2. Đối với mỗi chuỗi, lặp lại tất cả các chỉ số bắt đầu có thể có trong đó chuỗi con có độ dài 6 có thể tồn tại. Điều này có nghĩa là các chỉ số từ 0 đến len(s) - 6. Chúng tôi dừng việc xem xét các vị trí trước đó vì chúng không thể tạo thành một kết quả khớp hoàn toàn. 
3. Tại mỗi chỉ số bắt đầu, so sánh sáu ký tự tiếp theo với mẫu cố định “bitset”. Việc so sánh được thực hiện theo từng ký tự nên chúng ta có thể dừng ngay lập tức khi không khớp thay vì luôn kiểm tra tất cả sáu vị trí. 
4. Nếu tại bất kỳ thời điểm nào chúng tôi tìm thấy sự trùng khớp hoàn chỉnh, chúng tôi sẽ ngay lập tức đưa ra phản hồi khẳng định và chuyển sang trường hợp thử nghiệm tiếp theo. Việc thoát ra sớm chỉ quan trọng đối với hiệu quả vi mô ở đây, nhưng nó giữ cho logic rõ ràng. 
5. Nếu chúng tôi quét xong tất cả các vị trí hợp lệ mà không tìm thấy kết quả khớp, chúng tôi sẽ đưa ra phản hồi tiêu cực. 

### Tại sao nó hoạt động

Thuật toán kiểm tra toàn diện mọi vị trí liền kề có thể có của mẫu bên trong chuỗi. Bất kỳ lần xuất hiện hợp lệ nào cũng phải bắt đầu tại một số chỉ mục i và vòng lặp bao gồm mọi i như vậy chính xác một lần. Bởi vì mỗi chuỗi con ứng cử viên được so sánh chính xác với mẫu đầy đủ nên không có nguy cơ xảy ra kết quả dương tính giả. Ngược lại, nếu không tìm thấy kết quả trùng khớp thì đảm bảo rằng không có chuỗi con nào bằng chuỗi đích. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

TARGET = "bitset"
L = len(TARGET)

t = int(input())
for _ in range(t):
    s = input().strip()
    n = len(s)

    found = False
    for i in range(n - L + 1):
        if s[i:i+L] == TARGET:
            found = True
            break

    if found:
        print("7as")
    else:
        print("no 7as for you today")
```Giải pháp dựa vào việc cắt Python để so sánh các chuỗi con một cách hiệu quả và rõ ràng. Ranh giới vòng lặp`n - L + 1`đảm bảo chúng ta không bao giờ đọc quá cuối chuỗi. Điều này tránh các lỗi chỉ mục và tránh các kiểm tra không cần thiết khi không thể khớp đầy đủ. 

Cờ boolean`found`nắm bắt xem liệu chúng tôi đã gặp phải sự cố hợp lệ hay chưa. Sau khi thiết lập xong, chúng tôi nghỉ sớm để tránh so sánh dư thừa. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai đầu vào đại diện. 

### Ví dụ 1:`"fofolovesbitset"`| tôi | chuỗi con s[i:i+6] | khớp với “bitset”? | tìm thấy | 
| --- | --- | --- | --- | 
| 0 | fofolo | không | Sai | 
| 1 | ofolov | không | Sai | 
| 2 | ngu xuẩn | không | Sai | 
| 3 | ô liu | không | Sai | 
| 4 | yêub | không | Sai | 
| 5 | ovesbi | không | Sai | 
| 6 | vesbit | không | Sai | 
| 7 | esbits | không | Sai | 
| 8 | sbitse | không | Sai | 
| 9 | bitet | vâng | Đúng | 

Tại chỉ số 9, mẫu chính xác xuất hiện nên thuật toán dừng ngay lập tức và đưa ra kết quả khẳng định. Điều này chứng tỏ rằng các kết quả khớp một phần trước đó trong chuỗi không ảnh hưởng đến tính chính xác. 

### Ví dụ 2:`"bitnoset"`| tôi | chuỗi con s[i:i+6] | khớp với “bitset”? | tìm thấy | 
| --- | --- | --- | --- | 
| 0 | bitnos | không | Sai | 
| 1 | mũi | không | Sai | 
| 2 | tnoset | không | Sai | 

Không có kết quả phù hợp đầy đủ được tìm thấy. Mặc dù chuỗi trông giống với mẫu nhưng sự không khớp ở ký tự thứ tư sẽ cản trở việc chấp nhận. 

Điều này xác nhận rằng tính đúng đắn phụ thuộc vào sự bình đẳng chính xác liền kề chứ không phải sự giống nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t · n · 6) | Mỗi chuỗi trong số tối đa 100 chuỗi có độ dài 26 được quét trên tất cả các vị trí bắt đầu hợp lệ, với các so sánh có độ dài không đổi | 
| Không gian | O(1) | Chỉ có một số biến được sử dụng ngoài bộ lưu trữ đầu vào | 

Tổng số so sánh ký tự bị giới hạn bởi khoảng 100 × 26 × 6, con số này không đáng kể trong bất kỳ giới hạn thời gian thông thường nào. Giải pháp này hiệu quả một cách thoải mái ngay cả trong Python được thông dịch. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    TARGET = "bitset"
    L = len(TARGET)

    t = int(input())
    out = []
    for _ in range(t):
        s = input().strip()
        n = len(s)

        found = False
        for i in range(n - L + 1):
            if s[i:i+L] == TARGET:
                found = True
                break

        if found:
            out.append("7as")
        else:
            out.append("no 7as for you today")

    return "\n".join(out)

# provided samples
assert run("""6
fofolovesbitset
obitsetobitseto
bitnoset
blueblisteringbarnacles
thearkoffz
bitset
""") == """7as
7as
no 7as for you today
no 7as for you today
no 7as for you today
7as"""

# minimum size, no match
assert run("""1
a""") == "no 7as for you today"

# exact match only
assert run("""1
bitset""") == "7as"

# repeated pattern
assert run("""1
bitsetbitset""") == "7as"

# close but wrong
assert run("""1
bitsets""") == "7as"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ký tự đơn | không có 7as cho bạn ngày hôm nay | xử lý đầu vào tối thiểu | 
| chính xác “bitset” | 7s | trường hợp trận đấu trực tiếp | 
| lặp đi lặp lại “bitsetbitset” | 7s | nhiều lần xuất hiện | 
| “bitset” | 7s | hành vi chồng chéo ranh giới | 

## Vỏ cạnh 

Trường hợp một cạnh là khi độ dài chuỗi chính xác bằng 6. Trong trường hợp này, vòng lặp chỉ chạy một lần, ở chỉ số 0. Nếu chuỗi chính xác là “bitset”, thuật toán sẽ chấp nhận nó ngay lập tức; nếu không nó sẽ từ chối nó sau một lần so sánh. Điều này tránh mọi nguy cơ lập chỉ mục không hợp lệ. 

Một trường hợp khác là chuỗi ngắn hơn 6 ký tự. Ở đây, phạm vi vòng lặp trở nên trống vì`n - 6 + 1 <= 0`, do đó không có sự lặp lại nào xảy ra. Thuật toán trực tiếp đưa ra phản hồi phủ định, điều này đúng vì chuỗi ngắn hơn không thể chứa mẫu có độ dài-6. 

Trường hợp cuối cùng là các chuỗi chứa nhiều phần chồng chéo một phần như “bitbits et” (phân mảnh về mặt khái niệm). Thuật toán không cố gắng hợp nhất hoặc căn chỉnh các kết quả khớp một phần, vì vậy nó chỉ chấp nhận các kết quả khớp chính xác liền kề. Điều này đảm bảo tính chính xác ngay cả trong các cấu trúc đối nghịch trong đó các ký tự giống với mẫu nhưng bị lệch.
