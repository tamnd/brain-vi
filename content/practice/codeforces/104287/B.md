---
title: "CF 104287B - Leo Núi Dễ Dàng"
description: "Chúng ta được cung cấp một chuỗi độ cao dọc theo một con đường và chúng ta đi qua nó từ trái sang phải. Nhiệm vụ là đếm xem có bao nhiêu lần “leo núi” xuất hiện trong chuỗi này."
date: "2026-07-01T20:44:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104287
codeforces_index: "B"
codeforces_contest_name: "Teamscode Spring 2023 Contest"
rating: 0
weight: 104287
solve_time_s: 68
verified: true
draft: false
---

[CF 104287B - Leo núi dễ dàng](https://codeforces.com/problemset/problem/104287/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi độ cao dọc theo một con đường và chúng ta đi qua nó từ trái sang phải. Nhiệm vụ là đếm xem có bao nhiêu lần “leo núi” xuất hiện trong chuỗi này. 

Leo núi là bất kỳ đoạn tiếp giáp nào có ít nhất ba điểm liên tiếp trong đó độ cao tăng dần ở mỗi bước. Khi một phân đoạn tăng dần bắt đầu, nó sẽ tiếp tục miễn là mỗi giá trị tiếp theo lớn hơn giá trị trước đó. Nếu tiếp tục tăng hơn ba điểm thì vẫn tính là một ngọn núi duy nhất, không tính nhiều ngọn chồng lên nhau. Ý tưởng chính là chúng tôi đang tính các lần chạy tăng dần tối đa có độ dài ít nhất là ba. 

Kích thước đầu vào tối đa là 1000, đủ nhỏ để giải pháp O(N²) vẫn có thể vượt qua một cách thoải mái. Tuy nhiên, vì cấu trúc là tuyến tính và cục bộ nên chúng ta mong đợi việc quét O(N) là đủ. 

Một số trường hợp đặc biệt quan trọng ở đây. Một bước tăng lên không đủ để tạo thành một ngọn núi. Ví dụ, trong`[1, 2]`, không có gì được tính vì độ dài chạy chỉ là 2, mặc dù nó đang tăng lên. 

Nếu một dãy tăng rồi ngừng tăng thì chúng ta chỉ phải đếm nó một lần nếu nó đủ dài. Ví dụ,`[1, 2, 3, 4, 2]`chứa chính xác một ngọn núi, không có nhiều ngọn chồng lên nhau như`[1,2,3]`Và`[2,3,4]`. 

Giá trị phẳng phá vỡ một ngọn núi. Ví dụ,`[1, 2, 2, 3, 4]`đặt lại hoạt động tăng dần ở mức cao nhất, vì vậy chúng tôi không thể coi đó là một lần leo núi liên tục. 

## Phương pháp tiếp cận 

Một cách đơn giản để suy nghĩ về vấn đề này là kiểm tra mọi chỉ số ban đầu có thể có và mở rộng nó về phía trước trong khi dãy tăng nghiêm ngặt. Mỗi khi chúng tôi đạt đến điểm dừng tăng, chúng tôi sẽ kiểm tra xem độ dài đoạn có ít nhất là 3 hay không và đếm nếu có. 

Phương pháp brute-force này hoạt động vì mỗi ngọn núi là một mảng con tăng cực đại, do đó việc quét từ mỗi vị trí và mở rộng đảm bảo chúng ta không bỏ sót bất kỳ ứng cử viên nào. Tuy nhiên, thật lãng phí: đối với mỗi vị trí bắt đầu, chúng ta có thể quét tới tối đa O(N) phần tử, dẫn đến các phép toán O(N²) trong trường hợp xấu nhất, chẳng hạn như một mảng tăng đầy đủ. 

Điều quan trọng là chúng ta không cần phải khởi động lại quá trình quét ở mọi chỉ mục. Thay vào đó, chúng ta chỉ cần theo dõi các lần chạy tăng dần liền kề một cách chặt chẽ. Mỗi lần chạy có thể được xử lý một lần: khi chúng tôi phát hiện chuỗi ngừng tăng, chúng tôi sẽ hoàn thiện độ dài lần chạy và quyết định xem nó có tạo thành một ngọn núi hay không. 

Điều này biến vấn đề thành một bước duy nhất trong đó chúng tôi duy trì độ dài của chuỗi tăng dần hiện tại và đặt lại nó bất cứ khi nào chuỗi bị đứt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Quá chậm | 
| Theo dõi một lượt | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi quét mảng từ trái sang phải trong khi vẫn duy trì độ dài của đoạn tăng dần hiện tại. 

1. Bắt đầu với bộ đếm`cnt = 1`đại diện cho thời lượng chạy tăng dần hiện tại. 

Điều này hiệu quả vì một phần tử đơn lẻ có độ dài một phần tử. 
2. Đối với mỗi chỉ số`i`từ 1 đến N−1, so sánh`a[i]`với`a[i−1]`. 

Nếu như`a[i] > a[i−1]`, kéo dài thời gian chạy hiện tại bằng cách tăng`cnt`bởi một. 

Điều này bảo tồn tài sản mà`cnt`luôn đại diện cho độ dài của hậu tố tăng dần hiện tại. 
3. Nếu`a[i] <= a[i−1]`, tài sản bị phá vỡ ngày càng tăng. Trước khi đặt lại, hãy kiểm tra xem lượt chạy mà chúng ta vừa kết thúc có độ dài ít nhất là 3 hay không. Nếu vậy, hãy tăng câu trả lời. 

Sau đó đặt lại`cnt = 1`bởi vì lần chạy mới bắt đầu ở vị trí`i`. 
4. Sau khi kết thúc vòng lặp, chúng ta phải thực hiện kiểm tra lần cuối ở lần chạy cuối cùng, vì mảng có thể kết thúc trong khi vẫn tăng. Nếu như`cnt >= 3`, hãy coi nó như một ngọn núi. 

Lý do nó hoạt động xuất phát từ thực tế là mọi phân đoạn tăng nghiêm ngặt đều được xử lý chính xác một lần tại ranh giới của nó. Thuật toán không bao giờ chia tách một ngọn núi hợp lệ vì nó chỉ hoàn thành một đoạn khi thuộc tính tăng dần bị phá vỡ. Mỗi lần chạy tăng tối đa được xác định duy nhất và độ dài của nó xác định đầy đủ liệu nó có được tính hay không. Không có phân đoạn chồng chéo hoặc một phần nào được tính hai lần vì chúng tôi không bao giờ khởi động lại trong một chuỗi tăng hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

ans = 0
cnt = 1

for i in range(1, n):
    if a[i] > a[i - 1]:
        cnt += 1
    else:
        if cnt >= 3:
            ans += 1
        cnt = 1

if cnt >= 3:
    ans += 1

print(ans)
```Giải pháp giữ một bộ đếm hoạt động duy nhất cho chuỗi ngày càng tăng hiện tại. Mỗi khi trật tự bị phá vỡ, chúng tôi sẽ quyết định xem chuỗi đó có đủ tiêu chuẩn là một ngọn núi hay không. Việc kiểm tra cuối cùng sau vòng lặp đảm bảo đoạn cuối không bị bỏ sót. 

Sự tinh tế duy nhất là xử lý chính xác việc đặt lại:`cnt`phải luôn đặt lại về 1 chứ không phải 0 vì phần tử hiện tại bắt đầu một phân khúc tiềm năng mới. Quên lần kiểm tra cuối cùng là một lỗi phổ biến khác, vì lần chạy cuối cùng không bao giờ gây ra tình trạng ngắt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
11
1 2 3 2 4 4 1 4 5 7 3
```Chúng tôi theo dõi hoạt động chạy: 

| tôi | một [tôi] | trước | tăng dần? | cnt | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | - | bắt đầu | 1 | 0 | 
| 1 | 2 | 1 | vâng | 2 | 0 | 
| 2 | 3 | 2 | vâng | 3 | 0 | 
| 3 | 2 | 3 | không | 1 | 1 | 
| 4 | 4 | 2 | vâng | 2 | 1 | 
| 5 | 4 | 4 | không | 1 | 1 | 
| 6 | 1 | 4 | không | 1 | 1 | 
| 7 | 4 | 1 | vâng | 2 | 1 | 
| 8 | 5 | 4 | vâng | 3 | 1 | 
| 9 | 7 | 5 | vâng | 4 | 1 | 
| 10 | 3 | 7 | không | 1 | 2 | 

Cuối cùng, lần chạy cuối cùng`[1,4,5,7]`có chiều dài 4 và được tính. Đáp án cuối cùng là 2, tương ứng với`[1,2,3]`Và`[1,4,5,7]`. 

### Ví dụ 2 

đầu vào:```
6
5 4 3 2 1 2
```| tôi | một [tôi] | trước | tăng dần? | cnt | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 5 | - | bắt đầu | 1 | 0 | 
| 1 | 4 | 5 | không | 1 | 0 | 
| 2 | 3 | 4 | không | 1 | 0 | 
| 3 | 2 | 3 | không | 1 | 0 | 
| 4 | 1 | 2 | không | 1 | 0 | 
| 5 | 2 | 1 | vâng | 2 | 0 | 

Không có đoạn nào đạt tới độ dài 3, vì vậy câu trả lời là 0. 

Điều này xác nhận rằng chỉ những lần tăng tối đa có độ dài vừa đủ mới được tính và những biến động ngắn không đóng góp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi phần tử được xử lý một lần trong một lần | 
| Không gian | O(1) | Chỉ có một số quầy được duy trì | 

Các ràng buộc cho phép tối đa 1000 phần tử, nhưng quét tuyến tính đã tối ưu và để lại nhiều biên độ. Ngay cả dưới những ràng buộc lớn hơn nhiều, cách tiếp cận tương tự vẫn có hiệu lực do cấu trúc một lần của nó. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    ans = 0
    cnt = 1

    for i in range(1, n):
        if a[i] > a[i - 1]:
            cnt += 1
        else:
            if cnt >= 3:
                ans += 1
            cnt = 1

    if cnt >= 3:
        ans += 1

    return str(ans)

# provided sample
assert run("11\n1 2 3 2 4 4 1 4 5 7 3\n") == "2"

# minimum increasing mountain
assert run("3\n1 2 3\n") == "1"

# no mountain due to plateau
assert run("5\n1 2 2 3 4\n") == "1"

# all decreasing
assert run("4\n4 3 2 1\n") == "0"

# multiple separate mountains
assert run("7\n1 2 3 1 2 3 4\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 | 1 | núi hợp lệ tối thiểu | 
| 1 2 2 3 4 | 1 | phá vỡ cao nguyên liên tục | 
| 4 3 2 1 | 0 | không tăng phân khúc | 
| 1 2 3 1 2 3 4 | 2 | nhiều lần chạy độc lập | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi mảng kết thúc trong một chuỗi tăng dần. Ví dụ, trong`[1, 2, 3]`, không có “ngắt” để kích hoạt việc đếm bên trong vòng lặp. Thuật toán xử lý việc này bằng cách thực hiện kiểm tra cuối cùng sau khi lặp lại. Chiều dài đường chạy là 3 nên được tính chính xác là một ngọn núi. 

Một trường hợp khác là tăng giảm xen kẽ như`[1, 3, 2, 4, 3, 5]`. Thuật toán đặt lại ở mỗi lần giảm và chỉ đếm các phân đoạn đạt độ dài 3. Mỗi phân đoạn được cách ly rõ ràng và không xảy ra sự chồng chéo một phần vì`cnt`được thiết lập lại ngay lập tức khi tính đơn điệu không thành công. 

Một cao nguyên như`[1, 2, 2, 3]`được xử lý chính xác bởi vì`<=`điều kiện phá vỡ sự chạy ở mức bình đẳng. Phân khúc`[1, 2]`bị loại bỏ vì độ dài của nó nhỏ hơn 3 và lần chạy mới bắt đầu tại`2 -> 3`, cũng quá ngắn.
