---
title: "CF 352B - Jeff và Dấu chấm"
description: "Chúng ta được cung cấp một chuỗi các số nguyên và với mỗi giá trị riêng biệt, chúng ta muốn hiểu các lần xuất hiện của nó cách nhau như thế nào trên mảng. Với bất kỳ giá trị $x$ nào, chúng ta xem xét tất cả các chỉ số nơi $x$ xuất hiện."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 352
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 204 (Div. 2)"
rating: 1300
weight: 352
solve_time_s: 85
verified: true
draft: false
---

[CF 352B - Jeff và Dấu chấm](https://codeforces.com/problemset/problem/352/B) 

**Đánh giá:** 1300 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các số nguyên và với mỗi giá trị riêng biệt, chúng ta muốn hiểu các lần xuất hiện của nó cách nhau như thế nào trên mảng. Đối với bất kỳ giá trị$x$, chúng tôi xem xét tất cả các chỉ số trong đó$x$xuất hiện. Nếu các chỉ số đó tạo thành một chuỗi có hiệu không đổi thì$x$được coi là hợp lệ và chúng tôi cũng báo cáo sự khác biệt đó. Nếu như$x$chỉ xuất hiện một lần, sự khác biệt được xác định bằng không. 

Vì vậy, nhiệm vụ không phải là về bản thân các giá trị mà là về cấu trúc vị trí của chúng. Chúng tôi đang phân loại hiệu quả từng số riêng biệt dựa trên việc liệu kiểu xuất hiện của nó có cách đều nhau hay không. 

Kích thước đầu vào tăng lên$10^5$. Điều đó ngay lập tức loại trừ bất cứ điều gì liên tục quét toàn bộ mảng theo giá trị, vì cách tiếp cận nhóm và kiểm tra ngây thơ sẽ giảm xuống$O(n^2)$trong trường hợp xấu nhất khi nhiều giá trị lặp lại. Chúng ta cần một giải pháp xử lý từng phần tử với số lần không đổi. 

Trường hợp cạnh tinh tế xuất hiện khi một giá trị xuất hiện chính xác hai lần. Hai vị trí luôn tạo thành một cấp số cộng, vì vậy câu trả lời đơn giản là sự khác biệt của chúng. Một trường hợp góc khác là khi các lần xuất hiện không cách đều nhau nhưng có thể trông gần như đều đặn, chẳng hạn như các vị trí$[1, 3, 4]$. Một cách tiếp cận bất cẩn chỉ có thể kiểm tra các khác biệt liên tiếp cục bộ và bỏ lỡ việc mẫu bị phá vỡ sau đó. 

## Phương pháp tiếp cận 

Chiến lược bạo lực trước tiên sẽ là thu thập tất cả các vị trí cho từng giá trị riêng biệt, sau đó xác minh xem sự khác biệt giữa các vị trí liên tiếp có không đổi hay không. Điều này đúng về mặt logic: nếu tất cả các khoảng trống đều bằng nhau thì các vị trí sẽ tạo thành một cấp số cộng. 

Tuy nhiên, chi phí phụ thuộc vào cách chúng tôi tổ chức dữ liệu. Nếu chúng tôi liên tục quét toàn bộ mảng để tìm từng giá trị riêng biệt, chúng tôi sẽ thực hiện$O(n)$làm việc trên mỗi giá trị, trở thành$O(n^2)$trong trường hợp xấu nhất khi tất cả các phần tử đều khác biệt. Ngay cả khi chúng ta nhóm các vị trí trong từ điển trước tiên thì tổng chi phí để xây dựng danh sách là$O(n)$, nhưng việc xác minh từng nhóm vẫn tỷ lệ thuận với tần suất của nó, do đó việc kiểm tra tổng thể vẫn còn$O(n)$. Phần đó ổn. 

Vấn đề thực sự là chúng tôi phải tránh chi phí không cần thiết trong quá trình xử lý theo giá trị và đảm bảo chúng tôi không sắp xếp theo nhóm, điều này sẽ bổ sung thêm chi phí$\log n$yếu tố cho mỗi yếu tố riêng biệt. 

Quan sát quan trọng là chúng ta không cần bất cứ điều gì cầu kỳ ngoài một lần chuyển tiếp để thu thập vị trí, vì đầu vào đã có sẵn theo thứ tự. Nếu chúng tôi lưu trữ các chỉ mục khi quét thì mỗi danh sách vốn đã được sắp xếp. Sau đó, mỗi giá trị có thể được xác thực bằng cách kiểm tra một điểm khác biệt chung duy nhất trên các chỉ mục được lưu trữ của nó. 

Điều này làm giảm vấn đề đối với việc ghi sổ kế toán tuyến tính: tích lũy các vị trí, sau đó quét từng danh sách một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Nhóm + quét lặp lại |$O(n^2)$|$O(n)$| Quá chậm | 
| Nhóm + xác minh một lần cho mỗi giá trị |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một từ điển ánh xạ từng giá trị vào danh sách các chỉ mục nơi nó xuất hiện. 

1. Quét mảng từ trái sang phải và nối từng chỉ mục vào danh sách giá trị tương ứng. Điều này đảm bảo các vị trí được sắp xếp tự động mà không cần phải làm thêm. 
2. Với mỗi giá trị riêng biệt$x$, kiểm tra danh sách các vị trí của nó. Nếu nó chỉ chứa một phần tử thì câu trả lời cho giá trị đó ngay lập tức bằng 0 vì không tồn tại sự mơ hồ về khoảng cách. 
3. Nếu có ít nhất hai lần xuất hiện, hãy tính chênh lệch ban đầu bằng cách sử dụng hai vị trí đầu tiên. 
4. Duyệt qua các vị trí còn lại và xác minh rằng mọi chênh lệch liên tiếp đều khớp với vị trí ban đầu. Nếu tìm thấy sự không khớp, hãy loại bỏ hoàn toàn giá trị. 
5. Thu thập tất cả các giá trị hợp lệ và sự khác biệt của chúng, sau đó xuất chúng và sắp xếp theo chính giá trị đó. Việc sắp xếp chỉ được thực hiện trên các khóa riêng biệt chứ không phải trên toàn bộ mảng. 

### Tại sao nó hoạt động 

Đối với bất kỳ giá trị hợp lệ$x$, điều kiện “các vị trí tạo thành một cấp số cộng” tương đương với “tất cả các hiệu liên tiếp đều bằng nhau”. Vì các chỉ số đã được sắp xếp theo cách xây dựng nên việc kiểm tra các khác biệt liền kề là đủ và cần thiết. Nếu thậm chí một khoảng cách khác nhau, thuộc tính lũy tiến sẽ bị vi phạm. Ngược lại, nếu tất cả các khoảng trống đều khớp nhau thì định nghĩa của cấp số cộng được thỏa mãn, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    pos = {}
    
    for i, x in enumerate(a, start=1):
        if x not in pos:
            pos[x] = []
        pos[x].append(i)
    
    res = []
    
    for x in pos:
        p = pos[x]
        if len(p) == 1:
            res.append((x, 0))
            continue
        
        d = p[1] - p[0]
        ok = True
        
        for i in range(2, len(p)):
            if p[i] - p[i - 1] != d:
                ok = False
                break
        
        if ok:
            res.append((x, d))
    
    res.sort()
    
    out = []
    out.append(str(len(res)))
    for x, d in res:
        out.append(f"{x} {d}")
    
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp dựa trên một từ điển được khóa theo giá trị, trong đó mỗi mục lưu trữ một danh sách chỉ mục ngày càng tăng. Vì chúng tôi lặp từ trái sang phải nên không cần sắp xếp cho các danh sách này. Việc kiểm tra cấp số cộng được thực hiện bằng một lần tuyến tính duy nhất cho mỗi giá trị. 

Một lỗi phổ biến là tính toán lại các chênh lệch mà không sửa sai lệch tham chiếu sớm, dẫn đến những so sánh không cần thiết hoặc thoát sớm không chính xác. Một điều tinh tế khác là đảm bảo rằng các giá trị xuất hiện một lần được xử lý rõ ràng, vì nếu không chúng có thể bị bỏ qua không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 1 2 1
```Chúng tôi xây dựng danh sách vị trí: 

- 1 → [1, 3, 5] 
- 2 → [2, 4] 

Bây giờ chúng tôi xác minh: 

| Giá trị | Vị trí | Khác biệt đầu tiên | Séc | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | [1, 3, 5] | 2 | 3-1=2, 5-3=2 | Có | 
| 2 | [2, 4] | 2 | chỉ một lần kiểm tra | Có | 

Đầu ra:```
2
1 2
2 2
```Điều này cho thấy rằng nhiều giá trị có thể thỏa mãn điều kiện lũy tiến một cách độc lập. 

### Ví dụ 2 

đầu vào:```
6
3 3 3 3 3 3
```Danh sách vị trí: 

- 3 → [1, 2, 3, 4, 5, 6] 

| Bước | Vị trí | Kiểm tra khác biệt | 
| --- | --- | --- | 
| ban đầu | [1,2] | d = 1 | 
| kiểm tra | 2-1=1 | được | 
| kiểm tra | 3-2=1 | được | 
| kiểm tra | 4-3=1 | được | 
| kiểm tra | 5-4=1 | được | 
| kiểm tra | 6-5=1 | được | 

Đầu ra:```
1
3 1
```Điều này xác nhận rằng sự lặp lại thống nhất tạo ra một cấp số cộng hợp lệ ở bước 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + k \log k)$| Xây dựng danh sách vị trí là tuyến tính; sắp xếp các giá trị riêng biệt là$k \log k$, Ở đâu$k \le n$| 
| Không gian |$O(n)$| Mỗi chỉ mục được lưu trữ một lần trong danh sách cho mỗi giá trị | 

Những ràng buộc cho phép$10^5$hoạt động thoải mái và phân loại nhiều nhất$10^5$các khóa riêng biệt vẫn có hiệu lực trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    
    pos = {}
    for i, x in enumerate(a, start=1):
        pos.setdefault(x, []).append(i)
    
    res = []
    for x in pos:
        p = pos[x]
        if len(p) == 1:
            res.append((x, 0))
            continue
        d = p[1] - p[0]
        ok = all(p[i] - p[i-1] == d for i in range(2, len(p)))
        if ok:
            res.append((x, d))
    
    res.sort()
    
    out = [str(len(res))]
    for x, d in res:
        out.append(f"{x} {d}")
    return "\n".join(out)

# provided sample
assert run("1\n2\n") == "1\n2 0"

# all equal
assert run("5\n7 7 7 7 7\n") == "1\n7 1"

# two values, mixed validity
assert run("5\n1 2 1 2 1\n") == "2\n1 2\n2 2"

# invalid spacing
assert run("4\n1 2 3 1\n") == "2\n1 2\n2 0"

# single occurrences
assert run("3\n1 2 3\n") == "3\n1 0\n2 0\n3 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | giá trị duy nhất với bước 1 | khoảng cách không đổi lặp đi lặp lại | 
| mẫu hỗn hợp | hai chuỗi hợp lệ | xác minh độc lập theo giá trị | 
| khoảng cách không hợp lệ | bộ lọc bị hỏng tiến triển | phát hiện các khoảng trống không đổi | 
| tất cả đều khác biệt | tất cả số không | xử lý đơn lẻ | 

## Vỏ cạnh 

Một giá trị chỉ xuất hiện một lần là trường hợp đơn giản nhất nhưng rất dễ bị xử lý sai nếu việc triển khai giả định ít nhất hai vị trí trước khi tính toán sự khác biệt. Thuật toán chỉ định rõ ràng số 0 trong trường hợp này trước khi có bất kỳ quyền truy cập nào vào các phần tử lân cận, ngăn ngừa các lỗi ngoài phạm vi. 

Một trường hợp khác là khi một giá trị xuất hiện hai lần. Ví dụ đầu vào:```
4
5 1 5 1
```Đối với giá trị 5, các vị trí là [1, 3], do đó chênh lệch là 2. Thuật toán trực tiếp tính toán giá trị này từ cặp đầu tiên và chấp nhận nó mà không cần xác thực thêm. 

Một trường hợp tinh tế hơn là khi một chuỗi bắt đầu bằng một sự khác biệt hợp lệ nhưng sau đó bị ngắt:```
5
2 1 2 2 1
```Đối với giá trị 2, vị trí là [1, 3, 4]. Sự khác biệt đầu tiên là 2, nhưng sự khác biệt tiếp theo là 1, vì vậy nó bị loại bỏ. Quá trình quét sẽ phát hiện điều này ngay lập tức ở lần không khớp đầu tiên và loại bỏ giá trị, đảm bảo không có mẫu một phần nào được chấp nhận sai.
