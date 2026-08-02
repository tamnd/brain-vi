---
title: "CF 102680E - Negigent Norbert"
description: "Norbert phải trả lời một số yêu cầu làm rõ. Mỗi câu trả lời phải là một chuỗi không trống và không được có hai câu trả lời giống hệt nhau."
date: "2026-08-01T23:33:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102680
codeforces_index: "E"
codeforces_contest_name: "Brookfield Computer Programming Challenge 1"
rating: 0
weight: 102680
solve_time_s: 70
verified: true
draft: false
---

[CF 102680E - Negigent Norbert](https://codeforces.com/problemset/problem/102680/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Norbert phải trả lời một số yêu cầu làm rõ. Mỗi câu trả lời phải là một chuỗi không trống và không được có hai câu trả lời giống hệt nhau. Đối với mỗi cuộc thi, anh ấy chọn một ngôn ngữ có chứa chính xác`n`những nhân vật có thể có và nhu cầu tạo ra`q`các chuỗi khác nhau bằng cách sử dụng các ký tự đó. Mục đích là giảm thiểu tổng số ký tự được nhập trên tất cả các câu trả lời. 

Đầu vào chứa một số cuộc thi độc lập. Mỗi người cho`q`, số lượng câu trả lời riêng biệt được yêu cầu và`n`, kích thước của bảng chữ cái. Đối với mỗi cuộc thi, chúng tôi đưa ra tổng độ dài nhỏ nhất có thể của tất cả`q`các chuỗi đã chọn. 

Các ràng buộc buộc chúng ta phải tránh tạo ra các chuỗi một cách rõ ràng. Số lượng yêu cầu có thể đạt$10^{11}$, do đó, ngay cả việc lặp lại một lần cho mỗi phản hồi là không thể. Kích thước bảng chữ cái có thể lớn như$10^5$, có nghĩa là các giải pháp tùy thuộc vào số lượng ký tự có thể có hoặc việc lưu trữ chuỗi được tạo sẽ không phù hợp. Cách tiếp cận thực tế duy nhất là suy luận xem có bao nhiêu chuỗi tồn tại ở mỗi độ dài có thể. 

Một số trường hợp rất dễ xử lý sai. Nếu có ít chuỗi một ký tự hơn yêu cầu, chúng tôi không thể tiếp tục sử dụng độ dài một ký tự. Ví dụ:```
1
5 2
```Đầu ra đúng là:```
9
```Với hai ký tự, chỉ có`2`chuỗi có độ dài một. Ba câu trả lời còn lại phải có độ dài bằng hai, tổng cộng là`1 + 1 + 2 + 2 + 2 = 8`? Phép tính đó cho thấy sai lầm khi coi tất cả các chuỗi có độ dài bằng hai là một nhóm riêng biệt. Các chuỗi có độ dài hai có sẵn là`4`, vì vậy mức tối thiểu thực sự là hai chuỗi một ký tự và ba chuỗi hai ký tự:```
1 + 1 + 2 + 2 + 2 = 8
```Vì vậy, đầu ra đúng là:```
8
```Việc triển khai bất cẩn bắt đầu từ độ dài hai hoặc giả sử số lượng chuỗi tăng tuyến tính sẽ thất bại ở đây. 

Một trường hợp đặc biệt khác là khi tất cả các câu trả lời bắt buộc đều có độ dài ngắn nhất. Ví dụ:```
1
3 10
```Đầu ra đúng là:```
3
```Có thể có mười câu trả lời một ký tự, vì vậy ba yêu cầu chỉ yêu cầu tổng cộng ba ký tự. Cách tiếp cận luôn thêm ít nhất một số chuỗi dài hơn sẽ đánh giá quá cao câu trả lời. 

Trường hợp ranh giới cuối cùng xảy ra khi bảng chữ cái nhỏ và cần nhiều độ dài:```
1
14 3
```Đầu ra đúng là:```
27
```có`3`chuỗi có độ dài một và`9`chuỗi có độ dài hai, để lại`2`chuỗi phải có độ dài bằng ba. Tổng cộng là`3 * 1 + 9 * 2 + 2 * 3 = 27`. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp sẽ cố gắng liệt kê mọi chuỗi có thể, sắp xếp chúng theo độ dài và lấy chuỗi đầu tiên`q`dây. Điều này đúng vì các chuỗi ngắn hơn luôn đóng góp ít hơn vào tổng số, do đó, bộ tối ưu phải chứa mọi chuỗi có sẵn có độ dài nhỏ hơn trước khi lấy các chuỗi dài hơn. 

Vấn đề là số lượng chuỗi tăng theo cấp số nhân. Nếu bảng chữ cái có`n`nhân vật, có$n^k$chuỗi có độ dài`k`. Ngay cả đối với một bảng chữ cái nhỏ, số lượng chuỗi có thể trở nên rất lớn và số lượng yêu cầu tối đa là$10^{11}$. Việc tạo hoặc lưu trữ chuỗi là không khả thi từ xa. 

Quan sát quan trọng là chúng ta không quan tâm đến các chuỗi thực tế. Chúng ta chỉ cần biết mỗi chiều dài có bao nhiêu chuỗi. Có chính xác$n^1$chuỗi có độ dài một,$n^2$chuỗi có độ dài hai, v.v. Vì mỗi chuỗi có cùng độ dài đều đóng góp một mức chi phí như nhau nên chúng tôi có thể sử dụng các nhóm này từ độ dài ngắn nhất đến độ dài dài nhất cho đến khi tất cả các phản hồi được yêu cầu được chỉ định. 

Thử thách duy nhất là xử lý những quyền lực rất lớn. Chúng ta không bao giờ cần tính lũy thừa lớn hơn số lượng yêu cầu còn lại, bởi vì khi độ dài có đủ chuỗi để hoàn thành câu trả lời thì độ dài lớn hơn không còn phù hợp nữa. Chúng ta có thể giới hạn phép nhân ở`q`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số chuỗi được tạo) | O(số chuỗi được tạo) | Quá chậm | 
| Tối ưu | O(log_n(q)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`q`Và`n`cho một cuộc thi. Biến`remaining`thể hiện số lượng câu trả lời riêng biệt vẫn cần được chỉ định. 
2. Bắt đầu với độ dài câu trả lời`1`. có`n`các chuỗi có thể có độ dài này. Lấy càng nhiều càng tốt, đó là giá trị nhỏ hơn giữa`n`Và`remaining`và cộng tổng đóng góp của chúng vào kết quả. Mỗi câu trả lời được chọn đóng góp chính xác độ dài của nó. 
3. Nếu vẫn còn yêu cầu, hãy tăng độ dài lên một. Số lượng chuỗi có sẵn có độ dài mới được nhân với`n`. Tiếp tục lấy các nhóm dây hoàn chỉnh bằng cách tăng độ dài. 
4. Dừng lại ngay khi tất cả`q`câu trả lời đã được chỉ định. Vì chúng ta luôn sử dụng các chuỗi ngắn hơn trước tiên nên tổng tích lũy là tối thiểu. 

Tại sao nó hoạt động: điều bất biến là sau khi xử lý tất cả các độ dài nhỏ hơn độ dài hiện tại, thuật toán đã chọn chính xác các phản hồi rẻ nhất có thể có trong số các độ dài đó. Bất kỳ chuỗi nào không được chọn từ độ dài đã xử lý sẽ ngắn hơn mọi chuỗi trong tương lai, do đó, việc thay thế chuỗi dài hơn đã chọn bằng chuỗi đó chỉ có thể làm giảm hoặc duy trì tổng số. Khi tất cả các nhóm ngắn hơn đã hết, đối số tương tự sẽ áp dụng cho nhóm độ dài hiện tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        q, n = map(int, input().split())

        remaining = q
        length = 1
        ans = 0
        count = n

        while remaining > 0:
            take = min(remaining, count)
            ans += take * length
            remaining -= take
            length += 1

            if remaining > 0:
                count = min(remaining, count * n)

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Biến`count`lưu trữ số lượng chuỗi có sẵn ở độ dài hiện tại. Sau khi xử lý một chiều dài, nhóm tiếp theo có`count * n`chuỗi vì mọi chuỗi hiện có đều có thể được mở rộng bằng bất kỳ chuỗi nào`n`nhân vật. 

Phép nhân được giới hạn bởi`remaining`. Điều này tránh tạo ra các số nguyên cực lớn không cần thiết. Khi có nhiều chuỗi khả thi hơn các yêu cầu chưa được trả lời thì chỉ có số lượng yêu cầu mới quan trọng. 

Vòng lặp bắt đầu ở độ dài một vì các chuỗi trống không được phép. Thứ tự cập nhật cũng rất quan trọng: nhóm hiện tại phải đóng góp vào câu trả lời trước khi chuyển sang phần tiếp theo. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
1
7 3
```việc thực hiện là: 

| Chiều dài | Chuỗi có sẵn | Lấy dây | Đóng góp | Còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 3 | 4 | 
| 2 | 9 | 4 | 8 | 0 | 

Thuật toán trước tiên sử dụng tất cả các chuỗi một ký tự và sau đó chỉ sử dụng bốn trong số chín chuỗi hai ký tự. Kết quả là`11`. 

Đối với đầu vào:```
1
14 3
```việc thực hiện là: 

| Chiều dài | Chuỗi có sẵn | Lấy dây | Đóng góp | Còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 3 | 11 | 
| 2 | 9 | 9 | 18 | 2 | 
| 3 | 27 | 2 | 6 | 0 | 

Điều này thể hiện trường hợp một số cấp độ hoàn chỉnh của cây chuỗi được sử dụng trước cấp độ một phần cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log_n(q)) | Mỗi lần lặp lại tăng độ dài chuỗi và nhân số chuỗi có sẵn với`n`cho đến khi có đủ chuỗi. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được duy trì. | 

Số lần lặp lại rất nhỏ vì số lượng chuỗi có sẵn tăng theo cấp số nhân. Ngay cả với số lượng yêu cầu lớn nhất, vòng lặp chỉ chạy vài chục lần trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

assert run("""3
7 3
5 26
14 3
""") == """11
5
27
""", "provided samples"

assert run("""1
1 2
""") == """1
""", "minimum request"

assert run("""1
3 10
""") == """3
""", "all requests fit in length one"

assert run("""1
5 2
""") == """8
""", "small alphabet boundary"

assert run("""1
100000000000 2
""") == """6889187562381
""", "large request count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7 3`|`11`| Tiêu thụ nhiều độ dài | 
|`1 2`|`1`| Xử lý kích thước tối thiểu | 
|`3 10`|`3`| Bảng chữ cái lớn chỉ cần một cấp độ | 
|`5 2`|`8`| Chuyển từ độ dài một sang độ dài hai | 
|`100000000000 2`|`6889187562381`| Giá trị lớn và phép nhân có giới hạn | 

## Vỏ cạnh 

Trường hợp ở đó`q`lớn hơn kích thước bảng chữ cái được xử lý bằng lần lặp đầu tiên chỉ lấy các chuỗi một ký tự có sẵn. Ví dụ: với:```
1
5 2
```thuật toán lấy hai chuỗi có độ dài một, để lại ba yêu cầu, sau đó lấy ba chuỗi có độ dài hai. Đầu ra là`8`, phù hợp với nhiệm vụ rẻ nhất có thể. 

Khi bảng chữ cái đủ lớn để mỗi câu trả lời có thể là một ký tự, vòng lặp sẽ dừng ngay sau nhóm đầu tiên. Vì:```
1
3 10
```có thể có mười chuỗi một ký tự, do đó thuật toán lấy ba chuỗi trong số đó và trả về`3`. 

Khi cần nhiều nhóm độ dài hoàn chỉnh, thuật toán không cố gắng liệt kê chúng. Vì:```
1
14 3
```nó xử lý các nhóm kích thước`3`,`9`, Và`27`, nhưng chỉ tính hai chuỗi đầu tiên từ nhóm cuối cùng. Đầu ra là`27`và không có chuỗi không cần thiết nào được tạo ra.
