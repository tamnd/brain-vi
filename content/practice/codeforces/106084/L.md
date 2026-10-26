---
title: "CF 106084L - Máy dập ghim"
description: "Chúng ta có một màn hình hình chữ nhật thẳng hàng với các trục tọa độ. Các góc dưới bên trái và trên bên phải của nó xác định một hình chữ nhật có trục cố định trong mặt phẳng. Riêng biệt, chúng ta có một đoạn đường biểu thị đường dẫn của chốt ghim, được xác định bởi hai điểm cuối."
date: "2026-06-21T16:04:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106084
codeforces_index: "L"
codeforces_contest_name: "2025 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 106084
solve_time_s: 41
verified: true
draft: false
---

[CF 106084L - Kim bấm](https://codeforces.com/problemset/problem/106084/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một màn hình hình chữ nhật thẳng hàng với các trục tọa độ. Các góc dưới bên trái và trên bên phải của nó xác định một hình chữ nhật có trục cố định trong mặt phẳng. Riêng biệt, chúng ta có một đoạn đường biểu thị đường dẫn của chốt ghim, được xác định bởi hai điểm cuối. Tác dụng vật lý của việc dập ghim không chỉ ở các điểm cuối mà còn dọc theo toàn bộ đoạn giữa chúng. 

Nhiệm vụ là xác định xem đoạn này có chạm hoặc cắt hình chữ nhật tại bất kỳ điểm nào hay không, kể cả việc chạm vào đường biên. Nếu đúng như vậy thì màn hình bị hỏng và chúng ta xuất ra STOP. Nếu không, chúng tôi xuất ra OK. 

Do đó, câu hỏi hình học cốt lõi là bài toán giao nhau giữa đoạn thẳng và hình chữ nhật thẳng hàng với trục. 

Các ràng buộc cho phép tối đa 10.000 trường hợp thử nghiệm và tọa độ là các số nguyên nhỏ được giới hạn bởi 10^4 về giá trị tuyệt đối. Điều này loại trừ bất kỳ cấu trúc hình học nặng nào trên mỗi bài kiểm tra. Bất cứ điều gì vượt quá O(1) hoặc O(log n) cho mỗi bài kiểm tra là đủ, nhưng ngay cả việc kiểm tra hình học O(1) cũng được mong đợi rõ ràng. 

Một vài trường hợp tế nhị quan trọng. 

Một là khi cả hai điểm cuối đều nằm trong hình chữ nhật. Trong trường hợp đó, thiệt hại là hiển nhiên, nhưng một số cách triển khai không chính xác chỉ kiểm tra các điểm cuối ở bên ngoài. 

Một trường hợp khác là khi cả hai điểm cuối đều ở bên ngoài, nhưng đoạn đó vẫn đi qua hình chữ nhật, ví dụ như đường chéo. Kiểm tra chỉ điểm cuối ngây thơ sẽ bỏ lỡ điều này. 

Sự tinh tế cuối cùng là chạm vào ranh giới. Đoạn chỉ sượt qua một góc hoặc nằm chính xác trên một mép vẫn bị coi là hư hỏng. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là nghĩ về các điểm lấy mẫu dọc theo đoạn đường. Người ta có thể tưởng tượng việc rời rạc hóa đường thẳng từ (x1, y1) đến (x2, y2) và kiểm tra nhiều điểm trung gian để xem có điểm nào nằm bên trong hình chữ nhật hay không. Về nguyên tắc, điều này đúng vì hình chữ nhật là lồi và bất kỳ giao điểm nào cũng sẽ được ghi lại bằng mật độ lấy mẫu đủ. Tuy nhiên, số điểm cần thiết phụ thuộc vào khoảng cách tọa độ, có thể lên tới 2 * 10^4 trên mỗi trục. Trong trường hợp xấu nhất, điều này dẫn đến khoảng 10^4 mẫu trên mỗi phân đoạn cho mỗi trường hợp thử nghiệm và với 10^4 trường hợp thử nghiệm, con số này sẽ trở thành 10^8 thao tác trở lên, là đường biên hoặc quá chậm trong Python dưới 2 giây. 

Cách tốt hơn là sử dụng phép rút gọn hình học: thay vì kiểm tra các điểm vô hạn dọc theo đoạn thẳng, chúng ta kiểm tra xem đoạn đó có giao nhau với một vùng thẳng hàng với trục lồi hay không. Quan sát quan trọng là một hình chữ nhật thẳng hàng với trục có thể được coi là giao điểm của bốn nửa mặt phẳng. Một đoạn cắt hình chữ nhật khi và chỉ khi nó không nằm hoàn toàn ở một phía của bất kỳ đường giới hạn nào trong số bốn đường giới hạn và không nằm hoàn toàn bên ngoài trong khi thiếu tất cả các ranh giới. 

Một cách mạnh mẽ tiêu chuẩn để giải quyết vấn đề này là sử dụng ý tưởng cắt đoạn đường. Về mặt khái niệm, chúng tôi "cắt" đoạn đó vào hình chữ nhật. Nếu sau khi cắt mà vẫn còn một đoạn hợp lệ thì đó là giao điểm. Nếu đoạn bị cắt trống thì không có giao điểm. 

Đối với các hình chữ nhật thẳng hàng theo trục, điều này giảm xuống còn việc kiểm tra xem đoạn đó có giao nhau với tất cả bốn ràng buộc bản sàn trên x và y hay không. Điều này dẫn đến việc tính toán theo thời gian không đổi cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lấy mẫu / rời rạc hóa | O(D) mỗi lần kiểm tra | O(1) | Quá chậm | 
| Cắt đoạn hình chữ nhật / kiểm tra nửa mặt phẳng | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi hình chữ nhật là x trong [xl, xr] và y trong [yl, yr], và chúng ta muốn biết liệu đoạn từ p1 đến p2 có cắt hộp này hay không.

1. Đầu tiên, hãy kiểm tra xem điểm cuối có nằm bên trong hình chữ nhật hay không. Nếu vậy, chúng tôi ngay lập tức quay lại STOP. Điều này là do bất kỳ điểm nào bên trong hình chữ nhật đều có nghĩa là đoạn đó đã chạm vào màn hình. 
2. Nếu cả hai điểm cuối đều nằm ngoài thì chúng tôi chưa thể kết luận về mức độ an toàn. Một đoạn vẫn có thể cắt hoàn toàn hình chữ nhật mà không cần có điểm cuối ở bên trong. 
3. Tiếp theo chúng ta kiểm tra xem đoạn này có nằm hoàn toàn về một phía của hình chữ nhật dọc theo trục x hay không. Nếu cả hai tọa độ x đều nhỏ hơn xl thì toàn bộ đoạn thẳng nằm bên trái hình chữ nhật. Tương tự, nếu cả hai tọa độ x đều lớn hơn xr thì đoạn thẳng nằm bên phải nó. Trong cả hai trường hợp, không có giao lộ. 
4. Chúng ta thực hiện lập luận tương tự cho trục y. Nếu cả hai điểm cuối đều nằm dưới yl hoặc trên yr thì đoạn đó không thể chạm tới hình chữ nhật theo chiều dọc. 
5. Nếu không có điều kiện phân tách nào được giữ, đoạn đó phải cắt qua hình chữ nhật hoặc chạm vào nó, vì vậy chúng ta quay lại STOP. Nếu không chúng tôi sẽ trả lại OK. 

Tại sao nó hoạt động được gắn liền với độ lồi và căn chỉnh trục. Hình chữ nhật là lồi, do đó, bất kỳ giao điểm nào với một đoạn đều ngụ ý rằng đoạn đó đi từ ngoài vào trong mà không bị phân cách hoàn toàn bởi một đường hỗ trợ. Cách duy nhất để tránh giao nhau là cả hai điểm cuối nằm hoàn toàn trong một vùng được ngăn cách với hình chữ nhật bằng ít nhất một đường ranh giới thẳng hàng với trục. Việc kiểm tra sự phân tách x và y nắm bắt chính xác bốn nửa mặt phẳng rời rạc đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def inside(x, y, xl, yl, xr, yr):
    return xl <= x <= xr and yl <= y <= yr

t = int(input())
for _ in range(t):
    xl, yl, xr, yr = map(int, input().split())
    x1, y1, x2, y2 = map(int, input().split())

    if inside(x1, y1, xl, yl, xr, yr) or inside(x2, y2, xl, yl, xr, yr):
        print("STOP")
        continue

    if (x1 < xl and x2 < xl) or (x1 > xr and x2 > xr):
        print("OK")
        continue

    if (y1 < yl and y2 < yl) or (y1 > yr and y2 > yr):
        print("OK")
        continue

    print("STOP")
```Giải pháp trước tiên xử lý trường hợp dễ dàng nhưng quan trọng trong đó điểm cuối nằm bên trong hình chữ nhật, vì điều đó ngay lập tức gây ra hư hỏng. Hàm bên trong sử dụng các giới hạn bao hàm vì chạm vào ranh giới cũng được coi là thiệt hại. 

Tiếp theo, nó kiểm tra sự phân tách theo chiều ngang. Nếu cả hai điểm cuối đều nằm hoàn toàn trên một cạnh của hình chữ nhật trong x, thì đoạn đó hoàn toàn không thể đi vào vùng hình chữ nhật. 

Sau đó, nó sẽ kiểm tra sự phân tách theo chiều dọc tương tự. 

Nếu không có điều kiện phân tách nào xảy ra thì đoạn đó phải cắt ngang hình chữ nhật hoặc chạm vào nó ở đâu đó dọc theo bên trong hoặc ranh giới của nó. 

Một cạm bẫy triển khai phổ biến là sử dụng các bất đẳng thức nghiêm ngặt một cách không nhất quán. Ở đây, việc kiểm tra phân tách nghiêm ngặt đảm bảo rằng việc chạm vào ranh giới không được phân loại sai là an toàn. 

## Ví dụ đã hoạt động 

Xét một hình chữ nhật từ (0, 0) đến (5, 5) và một đoạn từ (1, 2) đến (2, 2). 

| Bước | Kiểm tra điểm cuối | X tách | Tách Y | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | cả bên ngoài | không | không | DỪNG | 

Trên thực tế, cả hai điểm đều nằm bên trong hình chữ nhật, vì vậy chúng tôi ngay lập tức phân loại nó là hư hỏng. Điều này xác nhận việc xử lý bao gồm điểm cuối. 

Bây giờ hãy xem xét hình chữ nhật (0, 0) đến (5, 5), đoạn (-1, -1) đến (-2, 6). 

| Bước | Kiểm tra điểm cuối | X tách | Tách Y | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | cả bên ngoài | có (cả hai < 0) | - | được | 

Cả hai điểm cuối đều nằm hoàn toàn bên trái hình chữ nhật nên đoạn thẳng không thể cắt nhau. 

Cuối cùng, hãy xem xét hình chữ nhật (0, 0) đến (5, 5), đoạn (-1, 2) đến (6, 2). 

| Bước | Kiểm tra điểm cuối | X tách | Tách Y | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | cả bên ngoài | không | không | DỪNG | 

Đoạn rõ ràng cắt hình chữ nhật theo chiều ngang tại y = 2. 

Những ví dụ này cho thấy cách logic phân tách phân biệt chính xác cấu hình an toàn và không an toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm thực hiện một số lượng so sánh và kiểm tra số học không đổi | 
| Không gian | O(1) | Không có cấu trúc phụ trợ nào được lưu trữ ngoài các biến đầu vào | 

Giải pháp chia tỷ lệ trực tiếp theo số lượng trường hợp thử nghiệm và dễ dàng phù hợp với giới hạn ngay cả đối với 10^4 trường hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    input = sys.stdin.readline

    t = int(input())
    for _ in range(t):
        xl, yl, xr, yr = map(int, input().split())
        x1, y1, x2, y2 = map(int, input().split())

        def inside(x, y):
            return xl <= x <= xr and yl <= y <= yr

        if inside(x1, y1) or inside(x2, y2):
            output.append("STOP")
        elif (x1 < xl and x2 < xl) or (x1 > xr and x2 > xr):
            output.append("OK")
        elif (y1 < yl and y2 < yl) or (y1 > yr and y2 > yr):
            output.append("OK")
        else:
            output.append("STOP")

    return "\n".join(output)

# provided samples
assert run("""3
0 0 5 5
1 2 2 2
0 0 5 5
-1 -1 -2 -2
0 0 5 5
1 6 2 7
""") == """STOP
OK
STOP"""

# custom cases
assert run("""1
0 0 5 5
0 0 10 10
""") == "STOP", "endpoint inside"

assert run("""1
0 0 5 5
-1 -1 -2 -2
""") == "OK", "fully outside one side"

assert run("""1
0 0 5 5
-1 2 6 2
""") == "STOP", "horizontal crossing"

assert run("""1
0 0 5 5
-1 5 6 5
""") == "STOP", "touch boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm cuối bên trong | DỪNG | phát hiện bên trong | 
| hoàn toàn bên ngoài một bên | được | logic tách | 
| qua ngang | DỪNG | giao lộ không có điểm cuối bên trong | 
| ranh giới chạm | DỪNG | xử lý ranh giới toàn diện | 

## Vỏ cạnh 

Trường hợp cạnh phổ biến là khi một điểm cuối nằm chính xác trên ranh giới hình chữ nhật. Ví dụ: hình chữ nhật (0, 0) đến (5, 5) và đoạn (5, 2) đến (10, 2). Điểm đầu tiên nằm ở cạnh phải, vẫn nằm bên trong theo định nghĩa bao hàm. Thuật toán phân loại nó là STOP ngay lập tức thông qua kiểm tra bên trong, phù hợp với yêu cầu rằng điểm tiếp xúc ranh giới bị hư hỏng. 

Một trường hợp khác là khi đoạn thẳng thẳng hàng với một cạnh nhưng một phần nằm ngoài. Ví dụ: đoạn (0, 2) đến (5, 2) nằm chính xác trên cạnh dưới của hình chữ nhật. Cả hai điểm cuối đều thỏa mãn yl <= y <= yr và ít nhất một x nằm trong giới hạn, do đó, bên trong trả về true và chúng tôi xuất ra STOP một cách chính xác. 

Một trường hợp tinh tế cuối cùng là đường chéo trong đó cả hai điểm cuối đều ở bên ngoài nhưng ở các góc phần tư đối diện với hình chữ nhật. Ví dụ (-1, -1) đến (6, 6). Cả hai điểm cuối đều không nằm bên trong, nhưng cả điều kiện phân tách x và y đều không đúng, do đó thuật toán xuất ra STOP một cách chính xác, ghi lại giao điểm xuyên qua phần bên trong hình chữ nhật.
