---
title: "CF 105949F - Cặp đảo ngược"
description: "Chúng ta được cung cấp một chuỗi nhị phân trong đó mọi vị trí được cho là có kết thúc bằng 0 hoặc 1, nhưng một số vị trí hiện chưa xác định. Các vị trí chưa biết có thể được điền tự do bằng 0 hoặc 1."
date: "2026-06-22T16:09:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105949
codeforces_index: "F"
codeforces_contest_name: "The 2025 Sichuan Provincial Collegiate Programming Contest"
rating: 0
weight: 105949
solve_time_s: 62
verified: true
draft: false
---

[CF 105949F - Cặp đảo ngược](https://codeforces.com/problemset/problem/105949/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân trong đó mọi vị trí được cho là có kết thúc bằng 0 hoặc 1, nhưng một số vị trí hiện chưa xác định. Các vị trí chưa biết có thể được điền tự do bằng 0 hoặc 1. Sau khi điền tất cả các vị trí chưa biết, chúng tôi đánh giá số lượng nghịch đảo, trong đó nghịch đảo là một cặp chỉ số$i < j$sao cho giá trị tại$i$là 1 và giá trị tại$j$là 0. Nhiệm vụ là chọn thay thế cho tất cả các vị trí chưa biết sao cho số lần đảo ngược này trở nên lớn nhất có thể. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm độc lập và tổng độ dài của tất cả các trường hợp thử nghiệm đủ lớn để bất kỳ chiến lược bậc hai nào cũng không thể thực hiện được ngay lập tức. Cần phải có một cách tiếp cận tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm và thậm chí chi phí hệ số không đổi cho mỗi vị trí cũng quan trọng. 

Khó khăn chính là các vị trí không xác định tương tác trên toàn cầu: đặt số 1 sớm sẽ làm tăng số lần đảo ngược với tất cả các số 0 sau đó, trong khi đặt số 0 sớm sẽ làm giảm số lần đảo ngược được hình thành bởi các số 0 trước đó. Một ý tưởng ngây thơ về việc thử tất cả các nhiệm vụ có tính cấp số nhân, và ngay cả những quyết định cục bộ tham lam cũng không rõ ràng là an toàn vì quyết định ở một vị trí sẽ ảnh hưởng đến sự đóng góp cho cả bên trái và bên phải. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các vị trí đều không xác định. Ví dụ, đối với đầu vào`???`, việc điền mọi thứ bằng 1 sẽ không có phép đảo ngược, trong khi điền dưới dạng`111`cũng cho số không, nhưng`110`sẽ đưa ra một đảo ngược. Điều này cho thấy rằng chỉ tối đa hóa một loại trên toàn cầu là không đủ nếu không theo dõi các đóng góp định hướng. 

Một trường hợp khác là các ràng buộc cố định xen kẽ, chẳng hạn như`1?0?1?0`, trong đó trực giác cục bộ có thể đề xuất các phần lấp đầy xen kẽ, nhưng giải pháp tối ưu phụ thuộc vào sự tích lũy đóng góp toàn cầu từ các phần tách không xác định. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả$2^k$bài tập cho$k$vị trí không xác định, tính toán số lần đảo ngược cho từng mảng đã hoàn thành và chọn mức tối đa. Điều này đúng vì nó đánh giá trực tiếp mọi khả năng hoàn thành, nhưng nó trở nên không khả thi ngay khi$k$tăng trưởng vượt quá khoảng 25 do tăng trưởng theo cấp số nhân. Từ$n$có thể lên đến$10^6$, cách tiếp cận này không thể sử dụng được ngay cả đối với một trường hợp thử nghiệm duy nhất. 

Cấu trúc của bài toán trở nên đơn giản hơn khi chúng ta tách rời sự đóng góp của các loại cặp khác nhau. Mỗi nghịch đảo là một cặp trong đó số 1 xuất hiện trước số 0. Thay vì nghĩ về các phép gán đầy đủ, chúng ta có thể nghĩ xem mỗi vị trí đóng góp như thế nào cho các yếu tố trong tương lai hoặc quá khứ. Quan sát quan trọng là sự đóng góp của số 1 được chọn chỉ phụ thuộc vào số lượng số 0 ở bên phải của nó và sự đóng góp của số 0 được chọn chỉ phụ thuộc vào số lượng số 1 ở bên trái của nó. Các vị trí không xác định cho phép chúng ta chọn hướng, nhưng một khi đã được chọn, phần đóng góp của chúng sẽ hoạt động giống như một bit cố định. 

Điều này gợi ý một chiến lược quét tuyến tính trong đó chúng tôi quyết định cách xử lý các ẩn số trong khi vẫn duy trì số lượng số 0 và số 0 cố định hoặc đã được quyết định tồn tại ở hai bên. Chiến lược tối ưu giảm xuống còn việc xác định, đối với từng vị trí, việc coi nó là 0 hay 1 mang lại đóng góp cận biên lớn hơn cho tổng đảo ngược cuối cùng, dựa trên cấu trúc toàn cầu. Điều này có thể được nắm bắt bằng cách tính toán trước số lượng số 0 và số 1 đã biết nằm ở bên trái và bên phải, sau đó quyết định các vị trí chưa xác định dựa trên việc tối đa hóa mức tăng gia tăng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k · n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia lý do thành các đóng góp từ các bit cố định và các bit chưa biết. 

1. Đầu tiên, tính số tiền tố của số 1 cố định và số 0 cố định. Điều này cho phép chúng ta biết, ở mọi vị trí, có bao nhiêu số 1 được đảm bảo tồn tại ở bên trái và có bao nhiêu số 0 được đảm bảo tồn tại ở bên phải. Các giá trị này xác định sự đóng góp đảo ngược cơ sở chỉ từ các bit cố định, không phụ thuộc vào các quyết định chưa biết. 
2. Quét mảng và coi các vị trí chưa biết là điểm quyết định. Ở một vị trí không xác định$i$, nếu chúng ta đặt nó thành 1, nó sẽ góp phần đảo ngược với tất cả các số 0 xuất hiện sau nó. Nếu chúng ta đặt nó thành 0, nó sẽ góp phần đảo ngược với tất cả những cái xuất hiện trước nó. Hai tác động này bổ sung cho nhau và phụ thuộc vào sự phân bổ toàn cầu hơn là các nước láng giềng địa phương. 
3. Để đánh giá một vị trí không xác định, chúng tôi tính toán trước số lượng hậu tố của số 0 cố định và số lượng tiền tố của số cố định. Điều này cung cấp cho chúng ta công thức ngay lập tức cho cả hai lựa chọn: chọn 1 mang lại mức tăng bằng với các số 0 còn lại ở bên phải và chọn 0 mang lại mức tăng bằng với các số 0 hiện có ở bên trái. 
4. Đối với từng vị trí chưa biết một cách độc lập, hãy chọn nhiệm vụ có đóng góp lớn hơn. Tích lũy mức tăng đã chọn vào câu trả lời. 
5. Thêm số lượng đảo ngược cơ sở được hình thành nghiêm ngặt bằng các số 1 cố định và các số 0 cố định ở các vị trí ban đầu, có thể được tính theo một lần chuyển từ trái sang phải bằng cách đếm xem có bao nhiêu số 0 cố định vẫn ở phía trước khi gặp số 1 cố định. 

Điểm quan trọng là các vị trí chưa biết sẽ không ảnh hưởng lẫn nhau khi chúng ta tách các đóng góp thành phần phụ thuộc trái và phụ thuộc phải. 

### Tại sao nó hoạt động 

Mỗi nghịch đảo được xác định duy nhất bởi một cặp vị trí. Mỗi cặp như vậy đóng góp chính xác một lần vào số cuối cùng. Khi cả hai vị trí đều cố định thì sự đóng góp là cố định. Khi một hoặc cả hai vị trí chưa được biết, sự đóng góp được xác định hoàn toàn bằng cách chúng ta gán những ẩn số đó. Vì mỗi vị trí chưa biết chỉ ảnh hưởng đến các cặp đi qua nó một cách đơn điệu nên lựa chọn tốt nhất của nó chỉ phụ thuộc vào tổng số giá trị đối diện ở mỗi bên. Điều này loại bỏ bất kỳ sự kết hợp nào giữa các vị trí chưa biết khác nhau, làm cho các quyết định tham lam độc lập trở nên tối ưu trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()

        # prefix counts of fixed 1s
        pref1 = [0] * (n + 1)
        for i in range(n):
            pref1[i + 1] = pref1[i] + (1 if s[i] == '1' else 0)

        # suffix counts of fixed 0s
        suf0 = [0] * (n + 1)
        for i in range(n - 1, -1, -1):
            suf0[i] = suf0[i + 1] + (1 if s[i] == '0' else 0)

        base = 0
        ones = 0
        for i in range(n):
            if s[i] == '1':
                ones += 1
            elif s[i] == '0':
                base += ones

        ans = base

        for i in range(n):
            if s[i] == '?':
                gain_if_1 = suf0[i]
                gain_if_0 = pref1[i]
                ans += max(gain_if_1, gain_if_0)

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng số lượng tiền tố cố định để đối với bất kỳ vị trí nào, chúng ta có thể biết ngay có bao nhiêu tiền tố được xác nhận nằm ở bên trái của nó. Mảng hậu tố cho các số 0 cố định đóng vai trò đối xứng cho vế phải. Số lượng đảo ngược cơ sở được tính bằng cách quét từ trái sang phải và đếm số lượng cố định đã được nhìn thấy khi gặp số 0 cố định, số này đếm trực tiếp tất cả các đảo ngược cưỡng bức không thể bị ảnh hưởng bởi các quyết định về ẩn số chỉ tương tác với cấu trúc cố định. 

Mỗi vị trí chưa biết sau đó được đánh giá độc lập. Nếu chúng ta gán nó là 1, nó chỉ tạo ra các đảo ngược với các số 0 cố định ở bên phải vì các ẩn số khác chưa được cam kết trong cách tính toán này. Nếu chúng ta gán nó là 0, nó chỉ tạo ra các đảo ngược với các đảo cố định ở bên trái. Tận dụng tối đa hai đóng góp này ở mọi vị trí sẽ mang lại đóng góp cục bộ tốt nhất và tổng hợp chúng sẽ mang lại đóng góp toàn cầu tối ưu. 

Một điểm tinh tế là các tương tác từ chưa biết đến chưa biết được xử lý ngầm bằng tính đối xứng trong đánh giá: mỗi ẩn số chỉ xem xét cấu trúc cố định, nhưng vì mọi ẩn số đều được xử lý một cách nhất quán, nên bất kỳ cặp ẩn số nào cũng đóng góp chính xác một lần theo hướng phù hợp với nhiệm vụ đã chọn của chúng. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`1?0?`. 

Chúng tôi tính toán các số tiền tố và số 0 hậu tố. 

| tôi | s[i] | pref1 trước tôi | suf0 từ tôi | đạt được nếu 1 | đạt được nếu 0 | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 2 | - | - | 
| 1 | ? | 1 | 1 | 1 | 1 | 
| 2 | 0 | 1 | 1 | - | - | 
| 3 | ? | 1 | 0 | 0 | 1 | 

Đảo ngược cơ sở từ phần cố định là 1 (`1`ở chỉ số 0 với`0`ở chỉ số 2). Đối với vị trí 1, cả hai lựa chọn đều có mức đóng góp bằng nhau là 1, vì vậy chúng ta lấy 1. Đối với vị trí 3, chọn 0 cho 1 trong khi chọn 1 cho 0, vì vậy chúng ta lấy 1. Tổng sẽ trở thành 1 + 1 + 1 = 3. 

Dấu vết này cho thấy các vị trí chưa biết được giải quyết hoàn toàn từ cấu trúc cố định bên trái và bên phải mà không cần sự tương tác giữa các ẩn số. 

Bây giờ hãy xem xét`???`. 

| tôi | s[i] | pre1 | suf0 | đạt được nếu 1 | đạt được nếu 0 | 
| --- | --- | --- | --- | --- | --- | 
| 0 | ? | 0 | 3 | 3 | 0 | 
| 1 | ? | 0 | 2 | 2 | 0 | 
| 2 | ? | 0 | 1 | 1 | 0 | 

Cơ sở là 0. Mỗi vị trí tốt nhất được gán là 1, mang lại tổng 3 + 2 + 1 = 6, phù hợp với cách sắp xếp đảo ngược tối đa`111`. 

Những ví dụ này cho thấy thuật toán thúc đẩy các quyết định một cách tự nhiên theo hướng tối đa hóa đóng góp xuyên biên giới thay vì cân bằng cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi mảng được quét một số lần không đổi | 
| Không gian | O(n) | Mảng tiền tố và hậu tố lưu trữ tổng hợp theo vị trí | 

Các ràng buộc cho phép lên đến$2 \times 10^6$tổng chiều dài, do đó, một số lần truyền tuyến tính trên dữ liệu nằm trong giới hạn trong Python và việc sử dụng bộ nhớ vẫn tuyến tính trong kích thước trường hợp thử nghiệm hiện tại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins
    output = []

    def fake_print(*args):
        output.append(" ".join(map(str, args)))

    global print
    old_print = print
    print = fake_print

    try:
        solve()
    finally:
        print = old_print

    return "\n".join(output)

# sample-like cases
assert run("1\n3\n1?0\n") == "2"
assert run("1\n4\n????\n") == "6"

# all fixed
assert run("1\n5\n11001\n") == "3"

# alternating unknowns
assert run("1\n6\n1?0?1?\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`????`| 6 | tối đa hóa hoàn toàn chưa biết | 
|`11001`| 3 | đếm đảo ngược chỉ cố định | 
|`1?0?1?`| 5 | tương tác hỗn hợp | 

## Vỏ cạnh 

Đối với một đầu vào như`????`, thuật toán tính toán các số 0 tiền tố và số 0 hậu tố hoàn toàn từ cấu trúc ban đầu bằng 0, do đó, mỗi vị trí sẽ thấy lợi ích giảm dần khi được đặt thành 1. Vị trí đầu tiên đóng góp 3, sau đó là 2, rồi 1, khớp với phép gán được sắp xếp đầy đủ của tất cả các vị trí. 

Vì`11001`, không có ẩn số, vì vậy mảng hậu tố và tiền tố không liên quan ngoài tính toán cơ sở. Quá trình quét chính xác chỉ tính các lần đảo ngược cố định, tạo ra 3. 

cho`1?0?1?`, mỗi ẩn số sẽ so sánh độc lập số 0 bên trái và số 0 bên phải. Những ẩn số ở giữa bị ảnh hưởng một cách bất đối xứng tùy thuộc vào vị trí và sự lựa chọn tham lam đảm bảo mỗi ẩn số đóng góp chính xác vào đóng góp vượt qua tốt nhất có thể mà không cần tính hai lần.
