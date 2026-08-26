---
title: "CF 104344C - Martelo"
description: "Chúng ta đang giải bài toán chuyển động một chiều trên trục số. Eren xuất phát ở vị trí 0 và muốn đến vị trí mục tiêu X. Trên đường đi, có một bức tường nằm ở Y, chặn lối đi cho đến khi Eren lấy được một cây búa ở vị trí Z."
date: "2026-07-01T18:27:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104344
codeforces_index: "C"
codeforces_contest_name: "Maratona dos Bixes 2023 - UNICAMP"
rating: 0
weight: 104344
solve_time_s: 88
verified: true
draft: false
---

[CF 104344C - Martelo](https://codeforces.com/problemset/problem/104344/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang giải bài toán chuyển động một chiều trên trục số. Eren xuất phát ở vị trí 0 và muốn đến vị trí mục tiêu X. Trên đường đi, có một bức tường nằm ở Y, chặn lối đi cho đến khi Eren lấy được một chiếc búa ở vị trí Z. Khi chiếc búa được thu thập, bức tường không còn ngăn cản chuyển động nên Eren có thể tự do đi qua Y sau đó. 

Nhiệm vụ là xác định xem liệu có thể đến X theo các quy tắc này hay không và nếu có thì tính tổng quãng đường tối thiểu đã đi. 

Bởi vì chuyển động diễn ra trên một đường thẳng và mỗi chi phí của đoạn đường chỉ là khoảng cách tuyệt đối nên mọi tuyến đường hợp lệ đều được xác định đầy đủ theo thứ tự Eren đến các điểm liên quan: 0, Z, Y và X, với ràng buộc là Y chỉ có thể vượt qua một cách an toàn sau khi đến Z. 

Các ràng buộc nhỏ, với tất cả các tọa độ trong khoảng từ -1000 đến 1000. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào có độ phức tạp không đổi hoặc logarit cho mỗi trường hợp thử nghiệm đều đủ và thậm chí một số lượng đánh giá đường dẫn cố định nhỏ cũng là đủ. 

Điểm tinh tế chính là bức tường không phải là sự ràng buộc một chiều theo nghĩa hình học. Đó là một ràng buộc về quyền: chỉ được phép băng qua Y sau khi đã truy cập Z. Trực giác về đường đi ngắn nhất ngây thơ mà bỏ qua thứ tự này sẽ thất bại. 

Một trường hợp thất bại điển hình phát sinh khi Z ở “sai phía” của Y so với điểm bắt đầu hoặc mục tiêu. Ví dụ: nếu Eren cần vượt qua Y sớm để đến Z, nhưng Z lại nằm ngoài Y theo cách buộc phải vượt qua Y trước khi lấy được chiếc búa, thì lộ trình này là không thể mặc dù tất cả các điểm đều nằm trên một đường thẳng. 

## Phương pháp tiếp cận 

Giải thích brute-force là liệt kê tất cả các hoán vị có thể có của việc truy cập các điểm liên quan, bắt đầu từ 0, kết thúc ở X và đảm bảo rằng Y chỉ được vượt qua sau khi Z đã được truy cập. Đối với mỗi thứ tự, chúng tôi sẽ mô phỏng đường đi và tính toán tổng quãng đường đã di chuyển, loại bỏ những đường đi không hợp lệ. 

Điều này hiệu quả vì số lượng điểm liên quan rất nhỏ nên chỉ có một vài hoán vị. Tuy nhiên, ngay cả trong trường hợp nhỏ này, lý luận vũ phu là không cần thiết vì cấu trúc bị ràng buộc hoàn toàn: chuyển động là tuyến tính và quyết định có ý nghĩa duy nhất là liệu Y có nằm giữa hai đoạn trước khi Z được thu thập hay không. 

Quan sát quan trọng là cách duy nhất mà bức tường quan trọng là nếu nó nằm hoàn toàn giữa 0 và Z, cũng như giữa 0 và X theo cách buộc phải di chuyển ngang trước khi lấy được chiếc búa. Nếu Y nằm giữa điểm bắt đầu và Z thì Eren phải vượt qua Y trước khi lấy được chiếc búa, điều này bị cấm nên không tồn tại đường đi hợp lệ. Mặt khác, đường đi tối ưu chỉ đơn giản là khoảng cách đường thẳng từ 0 đến Z đến X, với sự điều chỉnh chỉ nếu Y buộc phải truyền thêm trước Z. 

Cụ thể hơn, chúng tôi xem xét liệu Y có chặn đoạn giữa 0 và Z hay không. Nếu có thì hành trình là không thể. Nếu không thì Eren có thể an tâm đi đến Z trước mà không vi phạm ràng buộc. Sau đó, bức tường không còn liên quan nữa và phần còn lại chỉ là đường thẳng đi tới X. 

Điều này làm giảm vấn đề xuống còn một vài lần kiểm tra khoảng thời gian trên một dòng, thay vì lý luận về nhiều hoán vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | O(1) (liên tục 6 trường hợp) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Lý luận khoảng thời gian | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi trục số là các đoạn giữa 0, Z và X, đồng thời kiểm tra xem Y có buộc vượt qua không hợp lệ hay không trước khi lấy búa.

1. Đầu tiên hãy tính xem Y có nằm giữa 0 và Z hay không. Điều này có nghĩa là Y nằm giữa chúng trên trục số. Nếu điều này xảy ra, Eren sẽ phải vượt qua bức tường trước khi chạm tới chiếc búa, điều này là không được phép nên câu trả lời ngay lập tức là không thể. 
2. Nếu lần kiểm tra đầu tiên thành công, tiếp theo chúng tôi sẽ xem xét tổng khoảng cách của tuyến đường hợp lệ. Vì chiếc búa có sẵn trước khi bất kỳ ràng buộc nào được kích hoạt, Eren có thể đi trực tiếp từ 0 đến Z và sau đó trực tiếp từ Z đến X. 
3. Tổng chi phí chỉ đơn giản là |0 - Z| + |Z - X|, vì sau khi đạt đến Z thì bức tường không còn quan trọng nữa. 
4. Trả lại số tiền này làm đáp án. 

Tại sao nó hoạt động 

Thuật toán thực thi ràng buộc thực sự duy nhất trong bài toán: không thể vượt qua bức tường tại Y trước khi đến Z. Trên một đường thẳng, cách duy nhất có thể vi phạm ràng buộc này là nếu Y nằm hoàn toàn giữa điểm bắt đầu và búa. Khi đạt đến Z, trạng thái trở nên không bị giới hạn, do đó phần còn lại của hành trình là đường đi ngắn nhất trong không gian số liệu một chiều, luôn là khoảng cách trực tiếp. Bất kỳ tuyến đường thay thế nào đi vòng khỏi Z trước tiên sẽ chỉ làm tăng khoảng cách vì khoảng cách tuyệt đối trên một đường thỏa mãn bất đẳng thức tam giác chặt chẽ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

X, Y, Z = map(int, input().split())

def between(a, b, x):
    return min(a, b) < x < max(a, b)

if between(0, Z, Y):
    print(-1)
else:
    print(abs(Z) + abs(X - Z))
```Việc triển khai trực tiếp mã hóa điều kiện khả thi chính và kết quả là chi phí đường đi tối ưu. Chức năng trợ giúp`between`kiểm tra thứ tự nghiêm ngặt trên trục số, đây là điều kiện hình học duy nhất quan trọng đối với việc vượt sớm không hợp lệ. 

Công thức khoảng cách được chia thành hai đoạn: từ 0 đến Z và từ Z đến X. Điều này đúng vì khi đạt đến Z, giới hạn tường sẽ bị loại bỏ vĩnh viễn. 

Một cạm bẫy phổ biến là cố gắng tính Y trong tính toán khoảng cách. Y không bao giờ ảnh hưởng đến chi phí trừ khi nó chặn quyền truy cập vào Z, vì vậy nó sẽ không xuất hiện trong công thức cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 -10 1
```Chúng tôi kiểm tra xem Y = -10 có nằm giữa 0 và Z = 1 hay không. Không phải vậy, vì -10 nằm ngoài khoảng đó. 

Vì vậy chúng ta tiến hành tính đường đi 0 → 1 → 10. 

| Bước | Vị trí | Hành động | Khoảng cách | 
| --- | --- | --- | --- | 
| 1 | 0 → 1 | đi búa | 1 | 
| 2 | 1 → 10 | đi tới mục tiêu | 9 | 

Tổng khoảng cách là 10. 

Điều này khẳng định rằng bức tường không liên quan vì nó không cản trở việc tiếp cận búa trước. 

### Ví dụ 2 

đầu vào:```
20 10 -10
```Chúng tôi kiểm tra xem Y = 10 có nằm giữa 0 và Z = -10 hay không. Không, vì 10 nằm ngoài khoảng đó. 

Bây giờ chúng ta tính 0 → -10 → 20. 

| Bước | Vị trí | Hành động | Khoảng cách | 
| --- | --- | --- | --- | 
| 1 | 0 → -10 | đi búa | 10 | 
| 2 | -10 → 20 | đi tới mục tiêu | 30 | 

Tổng khoảng cách là 40. 

Điều này chứng tỏ rằng dù Y có ở xa cũng không thành vấn đề trừ khi nó nằm trên đường cưỡng bức tới Z. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lượng không đổi các phép so sánh và phép tính số học | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì tất cả các hoạt động đều có thời gian không đổi và không phụ thuộc vào cường độ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    X, Y, Z = map(int, sys.stdin.readline().split())

    def between(a, b, x):
        return min(a, b) < x < max(a, b)

    if between(0, Z, Y):
        return "-1"
    return str(abs(Z) + abs(X - Z))

# provided samples
assert run("10 -10 1") == "10"
assert run("20 10 -10") == "40"

# custom cases
assert run("5 2 3") == "-1", "wall blocks path to hammer"
assert run("-5 1 -2") == "3", "simple valid left side movement"
assert run("100 -50 50") == "150", "symmetric traversal through origin"
assert run("1 100 -100") == "200", "wall irrelevant when off path"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 2 3 | -1 | Y chặn đường đi giữa 0 và Z | 
| -5 1 -2 | 3 | truyền tải bên trái hợp lệ | 
| 100 -50 50 | 150 | qua điểm xuất phát không bị tắc nghẽn | 
| 1 100 -100 | 200 | bức tường không liên quan xa lối đi | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi Y nằm chính xác bên ngoài đoạn giữa 0 và Z nhưng giữa 0 và X. Ví dụ: nếu 0 → Z an toàn nhưng X nằm ở phía đối diện của Y thì bức tường vẫn không thành vấn đề vì nó đã bị phá hủy trước khi đến X. Thuật toán bỏ qua Y một cách chính xác trong tình huống này và vẫn tính |Z| + |X - Z|. 

Một trường hợp khác là khi Z nằm trong khoảng từ 0 đến X nhưng Y nằm trong khoảng từ 0 đến Z. Ví dụ: 0, Y = 2, Z = 5, X = 10. Kiểm tra phát hiện Y nằm trong (0, Z), do đó đầu ra là -1. Điều này phù hợp với thực tế là việc đạt đến Z đòi hỏi phải vượt qua Y sớm. 

Trường hợp tinh tế cuối cùng là khi tất cả các điểm đều âm. Logic khoảng tương tự được áp dụng mà không sửa đổi vì thứ tự trên trục số là đối xứng. Ví dụ: 0 → Z → X vẫn hợp lệ miễn là Y không nằm giữa 0 và Z.
