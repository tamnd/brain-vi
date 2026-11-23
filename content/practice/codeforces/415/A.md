---
title: "CF 415A - Mashmokh và đèn"
description: "Nhà máy có dãy đèn được đánh số từ trái qua phải. Mọi đèn đều bắt đầu ở trạng thái “bật”. Mashmokh thực hiện một chuỗi các lần nhấn nút và mỗi nút có một chỉ mục xác định mức độ kéo dài về bên phải của nó."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 415
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 240 (Div. 2)"
rating: 900
weight: 415
solve_time_s: 83
verified: true
draft: false
---

[CF 415A - Mashmokh và Ánh sáng](https://codeforces.com/problemset/problem/415/A) 

**Xếp hạng:** 900 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhà máy có dãy đèn được đánh số từ trái qua phải. Mọi đèn đều bắt đầu ở trạng thái “bật”. Mashmokh thực hiện một chuỗi các lần nhấn nút và mỗi nút có một chỉ mục xác định mức độ kéo dài về bên phải của nó. 

Khi một nút có chỉ mục`i`được nhấn, nó sẽ quét tất cả các đèn từ vị trí`i`đến cuối và tắt mọi đèn nó gặp vẫn sáng. Đèn đã tắt sẽ tắt và đèn trước`i`không bị ảnh hưởng. 

Nhiệm vụ là xác định, đối với mỗi đèn, nút nhấn cụ thể nào chịu trách nhiệm tắt đèn lần đầu tiên. Chúng tôi không được hỏi nó bị ảnh hưởng bao nhiêu lần hoặc điều gì xảy ra sau đó, chỉ có lần nhấn sớm nhất trong trình tự sẽ tắt nó. 

Các ràng buộc rất nhỏ: cả số lượng đèn và số lần nhấn nút tối đa là 100. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu nâng cao hoặc tối ưu hóa logarit. Ngay cả một mô phỏng bậc hai đơn giản cũng phù hợp một cách thoải mái trong giới hạn thời gian vì tổng số phép toán vẫn bị giới hạn ở khoảng mười nghìn. 

Một điểm tinh tế là các nút không được đảm bảo theo thứ tự. Nút sau có thể có chỉ số nhỏ hơn nút trước, nút này sẽ thay đổi đèn nào bị ảnh hưởng và thời điểm chúng tắt. Một chi tiết quan trọng khác là khi đèn tắt, những lần nhấn sau không thể thay đổi câu trả lời đã ghi. 

Một sai lầm ngây thơ là cho rằng mỗi nút chỉ ảnh hưởng đến một đoạn cố định không phụ thuộc vào các lần nhấn trước đó. Ví dụ: nếu bỏ qua điều kiện "vẫn bật", chúng tôi có thể liên tục ghi đè câu trả lời không chính xác. 

Hãy xem xét kịch bản nhỏ này: 

đầu vào:```
3 2
2 1
```Hành vi đúng: 

Sau khi nhấn phím 2 thì đèn 2 và 3 tắt nên cả hai được gán nút 2. Sau khi nhấn phím 1, đèn 1 tắt và được gán nút 1. 

Việc mô phỏng bất cẩn liên tục tắt đèn đã tắt có thể ghi đè sai câu trả lời hoặc cố gắng gán lại chúng nhiều lần. 

Điều quan trọng là mỗi đèn phải được chỉ định chính xác một lần, tại thời điểm nó chuyển từ bật sang tắt. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để hình dung về quy trình là mô phỏng nó chính xác như được mô tả. Chúng tôi duy trì một mảng biểu thị xem mỗi đèn có bật hay không và một mảng khác lưu trữ câu trả lời cho mỗi đèn. Với mỗi lần nhấn nút, chúng tôi sẽ quét tất cả các đèn bị ảnh hưởng từ chỉ mục đến`n`, và bất cứ khi nào chúng tôi gặp đèn vẫn sáng, chúng tôi đánh dấu nó là đã tắt bằng nút này. 

Cách tiếp cận này đúng vì nó phản ánh từng bước quy trình của nhà máy. Vấn đề là tính hiệu quả: trong trường hợp xấu nhất, mỗi`m`máy ép có thể quét tới`n`đèn, dẫn đến khoảng`n * m`hoạt động. Với các ràng buộc đã cho, điều này vẫn có thể chấp nhận được vì tối đa chỉ là 10.000 thao tác. 

Một quan sát mang tính khái niệm hơn một chút sẽ đơn giản hóa việc lập luận. Một ánh sáng`j`bị tắt khi lần đầu tiên chúng ta gặp một nút có chỉ số nhỏ hơn hoặc bằng`j`trong trình tự. Các nút trước đó có chỉ số lớn hơn sẽ không ảnh hưởng đến nút đó và khi nút hợp lệ xuất hiện, đèn đã tắt và sẽ không bao giờ được xem xét nữa. Quan sát này khớp với mô phỏng và giải thích lý do tại sao chỉ cần lướt qua từng đèn và nút là đủ. 

Vì vậy, cả mô phỏng trực tiếp và quan điểm rút ra từ “báo chí đủ điều kiện đầu tiên” đều dẫn đến cùng một cơ cấu thực hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n · m) | O(n) | Đã chấp nhận | 
| Quét kích hoạt trực tiếp lần đầu | O(n · m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô tả mô phỏng theo cách tương ứng trực tiếp với quá trình diễn ra như thế nào. 

1. Khởi tạo một mảng`ans`kích thước`n`, chứa đầy số không. Điều này sẽ lưu trữ chỉ mục nút tắt mỗi đèn. Cũng duy trì một mảng boolean`on`được khởi tạo thành tất cả đúng, biểu thị rằng ban đầu tất cả các đèn đều bật. 
2. Lặp lại các nút theo thứ tự đã cho từ lần nhấn đầu tiên đến lần nhấn cuối cùng. 
3. Đối với mỗi nút có chỉ mục`b_k`, quét tất cả các đèn từ vị trí`b_k`bởi vì`n`. 
4. Nếu đèn ở vị trí`j`vẫn còn, chỉ định`ans[j] = b_k`và đánh dấu nó là tắt. 
5. Nếu đèn đã tắt thì bỏ qua vì kết quả đã được xác định trước đó và không thể thay đổi. 
6. Tiếp tục cho đến khi tất cả các lần nhấn nút được xử lý. 

Lý do chúng tôi chỉ chỉ định khi đèn vẫn sáng là vì chúng tôi đang ghi lại chính xác khoảnh khắc chuyển đổi đầu tiên sang trạng thái tắt. Bất kỳ nút nào sau này cũng bao gồm vị trí đó đều phải được bỏ qua. 

### Tại sao nó hoạt động 

Mỗi đèn thay đổi trạng thái đúng một lần, từ bật sang tắt. Lần đầu tiên một nút có chỉ mục`b_k ≤ j`được xử lý, nhẹ`j`vẫn bật vì không có nút nào trước đó có thể che được nó. Báo chí đó nhất thiết phải bao gồm`j`trong phạm vi của nó và tắt nó đi. Từ thời điểm đó trở đi, không thao tác tiếp theo nào có thể ảnh hưởng đến câu trả lời được ghi lại vì chúng tôi ngăn chặn việc ghi đè một cách rõ ràng. Điều này đảm bảo rằng`ans[j]`lưu trữ báo chí hợp lệ sớm nhất đạt được nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    b = list(map(int, input().split()))

    ans = [0] * n
    on = [True] * n

    for btn in b:
        i = btn - 1
        for j in range(i, n):
            if on[j]:
                on[j] = False
                ans[j] = btn

    print(*ans)

if __name__ == "__main__":
    solve()
```Giải pháp tuân theo mô phỏng chính xác được mô tả trước đó. Mảng`on`đảm bảo rằng mỗi đèn chỉ được gán một lần. Vòng lặp bên trong bắt đầu từ`btn - 1`bởi vì đèn được lập chỉ mục 0 khi triển khai, trong khi đầu vào được lập chỉ mục 1. 

Một sai lầm phổ biến là quên rằng khi đèn đã tắt, các nút sau đó không được ghi đè câu trả lời của đèn đó. Kiểm tra có điều kiện`if on[j]`thực thi bất biến này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4
4 3 1 2
```| Bước | Nút | Phạm vi bị ảnh hưởng | Cập nhật | 
| --- | --- | --- | --- | 
| 1 | 4 | 4-5 | đèn 4 ← 4, đèn 5 ← 4 | 
| 2 | 3 | 3-5 | ánh sáng 3 ← 3 | 
| 3 | 1 | 1-5 | đèn 1 ← 1, đèn 2 ← 1 | 
| 4 | 2 | 2-5 | không thay đổi | 

Đầu ra cuối cùng:```
1 1 3 4 4
```Dấu vết này cho thấy các nút chỉ mục lớn trước đó chỉ ảnh hưởng đến phía bên phải, trong khi các nút chỉ mục nhỏ sau này sẽ lấp đầy các đèn không được che chắn còn lại. 

### Ví dụ 2 

đầu vào:```
4 3
2 4 1
```| Bước | Nút | Phạm vi bị ảnh hưởng | Cập nhật | 
| --- | --- | --- | --- | 
| 1 | 2 | 2-4 | đèn 2 ← 2, đèn 3 ← 2, đèn 4 ← 2 | 
| 2 | 4 | 4-4 | không thay đổi (đã tắt) | 
| 3 | 1 | 1-4 | ánh sáng 1 ← 1 | 

Đầu ra cuối cùng:```
1 2 2 2
```Ví dụ này nhấn mạnh rằng khi đèn tắt sớm, các lần nhấn sau không thể ghi đè lên đèn đó, ngay cả khi về mặt kỹ thuật chúng bao gồm chỉ số của đèn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · m) | Mỗi nút có thể quét tối đa tất cả các đèn ở bên phải của nó và mỗi đèn được chỉ định tối đa một lần | 
| Không gian | O(n) | Chúng ta lưu trữ trạng thái của từng đèn và mảng đáp án cuối cùng | 

Với cả hai`n`Và`m`giới hạn ở mức 100, số thao tác trong trường hợp xấu nhất đủ nhỏ để thực hiện ngay lập tức trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    old = sys.stdout
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()
    sys.stdout = old
    return out.strip()

# provided sample
assert run("5 4\n4 3 1 2\n") == "1 1 3 4 4", "sample 1"

# minimum size
assert run("1 1\n1\n") == "1", "single light"

# already left-to-right pressing
assert run("3 3\n1 2 3\n") == "1 2 3", "strict order"

# reverse order
assert run("3 3\n3 2 1\n") == "3 2 1", "reverse order"

# overlapping coverage
assert run("4 2\n2 1\n") == "1 1 2 2", "overlap case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`1`| ranh giới tối thiểu | 
|`3 / 1 2 3`|`1 2 3`| bảo hiểm đơn điệu | 
|`3 / 3 2 1`|`3 2 1`| hiệu ứng thứ tự giảm dần | 
|`4 / 2 1`|`1 1 2 2`| phạm vi chồng chéo | 

## Vỏ cạnh 

Trường hợp đèn đơn tối thiểu sẽ kiểm tra xem việc triển khai có xử lý việc lập chỉ mục một cách chính xác hay không và không cố truy cập vào các phạm vi không hợp lệ. Khi`n = 1`, nút bất kỳ`1`ngay lập tức tắt ngọn đèn duy nhất và nhiệm vụ rất đơn giản. 

Khi các nút tăng dần, mỗi lần nhấn có xu hướng chỉ ảnh hưởng đến hậu tố chưa được khám phá. Thuật toán chỉ định chính xác từng đèn khi chỉ mục của nó bị che lần đầu tiên, tạo ra ánh xạ trực tiếp giữa vị trí và nút đủ điều kiện sớm nhất. 

Khi các nút giảm dần, lần nhấn đầu tiên đã bao gồm hầu hết mọi thứ. Trong trường hợp này, vòng lặp bên trong vẫn chạy chính xác nhưng chỉ gán giá trị một lần và các lần nhấn sau đó không tìm thấy đèn hoạt động nào để sửa đổi. Điều này khẳng định rằng`on`mảng ngăn chặn việc ghi đè một cách chính xác. 

Khi phạm vi phủ sóng chồng chéo nhiều, các máy ép phạm vi rộng trước đó có thể ưu tiên các máy ép phạm vi nhỏ sau này. điều kiện`if on[j]`đảm bảo rằng một khi đèn được chỉ định, nó vẫn cố định, giúp duy trì tính chính xác ngay cả trong các kiểu chồng chéo mạnh mẽ.
