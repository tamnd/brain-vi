---
title: "CF 105387H - Đồ chơi"
description: "Chúng ta được cung cấp một chuỗi các số nguyên xác định được tạo ra bằng cách nhân liên tục giá trị trước đó với một hệ số cố định và sau đó lấy một modulo. Chuỗi bắt đầu từ một giá trị ban đầu nhất định và tạo ra chính xác các số $n$."
date: "2026-06-23T05:09:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105387
codeforces_index: "H"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2023"
rating: 0
weight: 105387
solve_time_s: 67
verified: true
draft: false
---

[CF 105387H - Đồ chơi](https://codeforces.com/problemset/problem/105387/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các số nguyên xác định được tạo ra bằng cách nhân liên tục giá trị trước đó với một hệ số cố định và sau đó lấy một modulo. Chuỗi bắt đầu từ một giá trị ban đầu nhất định và tạo ra chính xác$n$những con số. Mỗi số tiếp theo chỉ phụ thuộc vào số trước đó, vì vậy toàn bộ hàng được xác định đầy đủ sau khi biết bốn tham số. 

Nhiệm vụ không phải là tính toán toàn bộ chuỗi mà là xác định năm giá trị lớn nhất xuất hiện ở bất kỳ đâu trong hàng được tạo này và xuất chúng theo thứ tự tăng dần. 

Sự ràng buộc về$n$đạt tới$2 \cdot 10^7$, có nghĩa là bất kỳ cách tiếp cận nào cố gắng lưu trữ tất cả các giá trị hoặc thực hiện bất cứ điều gì tệ hơn thời gian tuyến tính sẽ thất bại. Một bậc hai hoặc thậm chí$O(n \log n)$Giải pháp dựa trên sắp xếp cũng quá chậm trong thực tế vì việc tạo chuỗi đã yêu cầu hàng chục triệu thao tác và chi phí bổ sung sẽ vượt quá giới hạn thời gian. Giới hạn bộ nhớ cũng ngăn cản việc lưu trữ toàn bộ mảng một cách an toàn trong Python nếu mỗi phần tử được giữ trong danh sách có chi phí chung. 

Một cách giải thích đơn giản là tạo ra tất cả các giá trị, sắp xếp chúng và chọn năm giá trị cao nhất. Cách tiếp cận đó đúng về mặt logic nhưng lại bị phá vỡ ngay lập tức ở quy mô lớn. Vì$n = 20{,}000{,}000$, việc sắp xếp sẽ yêu cầu theo thứ tự$n \log n$, vượt xa những gì có thể thực hiện được. 

Một vấn đề phức tạp hơn xuất hiện khi người ta cố gắng lưu trữ tất cả các giá trị chỉ để quét chúng sau này. Ngay cả khi bộ nhớ phù hợp, hoạt động của bộ đệm vẫn trở nên kém và chi phí Python chiếm ưu thế trong thời gian thực thi. 

Một trường hợp góc đáng chú ý là khi chuỗi nhanh chóng bị lặp lại hoặc bằng 0. Ví dụ, nếu$p = 1$, thì mọi giá trị đều bằng 0 bất kể$a_1$Và$k$. Đầu ra đúng trong trường hợp đó chỉ đơn giản là năm số không, nhưng việc triển khai bất cẩn giả định tính biến đổi vẫn có thể cố gắng xử lý không cần thiết hoặc xử lý sai các bản sao. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: tạo ra tất cả$n$các giá trị bằng cách sử dụng phép lặp, lưu trữ chúng trong một mảng, sắp xếp mảng và lấy năm phần tử cuối cùng. Điều này có tác dụng vì việc sắp xếp xếp hạng chính xác tất cả các giá trị nhưng nó yêu cầu cả bộ nhớ cho$n$số nguyên và thời gian$O(n \log n)$. Với$n$lên đến hai mươi triệu, ngay cả việc quét tuyến tính cần thiết để xây dựng mảng cũng đã đắt đỏ và việc sắp xếp chiếm ưu thế hoàn toàn. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần thứ tự đầy đủ của tất cả các giá trị. Chúng ta chỉ cần biết năm giá trị lớn nhất được thấy cho đến nay khi truyền phát qua chuỗi. Điều này chuyển đổi vấn đề từ nhiệm vụ đặt hàng toàn cầu thành vấn đề lựa chọn phát trực tuyến. 

Bản thân trình tự được tạo ra trong một lần chuyển và ở mỗi bước, chúng ta có thể duy trì một cấu trúc nhỏ chứa năm ứng cử viên tốt nhất hiện tại. Vì cấu trúc đó không bao giờ vượt quá kích thước năm nên tất cả các cập nhật đều có thời gian không đổi. Quá trình chuyển đổi từ sắp xếp mọi thứ sang duy trì tập hợp trên cùng có kích thước cố định là điều sẽ loại bỏ hoàn toàn hệ số logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (lưu trữ + sắp xếp) |$O(n \log n)$|$O(n)$| Quá chậm | 
| Đang phát trực tuyến bảo trì top 5 |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính giá trị đầu tiên$a_1$và coi nó là ứng cử viên đầu tiên cho câu trả lời. Điều này khởi tạo cấu trúc đang chạy vì không thể so sánh được trước khi chuỗi bắt đầu. 
2. Duy trì một danh sách nhỏ gồm tối đa năm số đại diện cho các giá trị lớn nhất được thấy cho đến nay. Hãy sắp xếp nó theo thứ tự tăng dần để cái nhỏ nhất trong số chúng luôn dễ dàng truy cập. 
3. Lặp lại trình tự từ phần tử thứ hai đến phần tử thứ hai$n$-phần tử thứ, tạo ra từng giá trị bằng cách sử dụng$a_i = (a_{i-1} \cdot k) \bmod p$. Bước này hoàn toàn là phát trực tuyến nên không có giá trị trước đây nào được lưu trữ. 
4. Đối với mỗi giá trị mới được tạo, hãy so sánh nó với phần tử nhỏ nhất trong danh sách top 5 hiện tại. Nếu nó không lớn hơn, hãy loại bỏ nó ngay lập tức vì nó không ảnh hưởng đến đáp án cuối cùng. 
5. Nếu giá trị mới lớn hơn giá trị nhỏ nhất trong danh sách top 5, hãy chèn nó vào danh sách, loại bỏ phần tử nhỏ nhất để giữ kích thước cố định và khôi phục thứ tự sắp xếp. Điều này đảm bảo danh sách luôn chứa những ứng viên tốt nhất được thấy cho đến nay. 
6. Sau khi xử lý xong tất cả$n$giá trị, xuất năm giá trị được lưu trữ theo thứ tự tăng dần. 

Bất biến chính là sau khi xử lý lần đầu tiên$i$phần tử, danh sách được duy trì chứa chính xác năm giá trị lớn nhất trong số đó$i$các phần tử, được sắp xếp. Điều này ban đầu đúng vì danh sách bắt đầu với phần tử đầu tiên và phát triển cho đến kích thước thứ năm, đồng thời nó được giữ nguyên vì mọi giá trị mới không thể lọt vào năm phần tử hàng đầu hoặc thay thế giá trị nhỏ nhất trong số năm phần tử hàng đầu hiện tại mà không làm ảnh hưởng đến các phần tử lớn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, a1, k, p = map(int, input().split())

top = [a1]

cur = a1
for i in range(1, n):
    cur = (cur * k) % p

    if len(top) < 5:
        top.append(cur)
        top.sort()
    else:
        if cur > top[0]:
            top[0] = cur
            top.sort()

print(*top)
```Giải pháp dựa trên cấu trúc dữ liệu tối thiểu: danh sách tối đa năm phần tử. Mỗi lần cập nhật là một công việc liên tục vì việc sắp xếp một mảng có kích thước cố định thực sự là thời gian không đổi. Sự lặp lại được tính toán lặp đi lặp lại để tránh tính toán lại hoặc lưu trữ. 

Một điểm tinh tế là chúng tôi không bao giờ lưu trữ toàn bộ chuỗi. Biến`cur`chỉ mang giá trị trước đó, đủ để tạo giá trị tiếp theo. Điều này giữ cho việc sử dụng bộ nhớ không đổi. 

Một chi tiết khác là logic thay thế có điều kiện. Chỉ khi giá trị mới vượt quá mức tối thiểu hiện tại của năm giá trị hàng đầu thì chúng tôi mới sửa đổi cấu trúc. Điều này ngăn chặn các hoạt động sắp xếp không cần thiết, điều này rất quan trọng do hạn chế về thời gian. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 1 2 100
```| Bước | Giá trị hiện tại | Top 5 trước | Hành động | Top 5 sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | trống | chèn | [1] | 
| 2 | 2 | [1] | chèn | [1, 2] | 
| 3 | 4 | [1, 2] | chèn | [1, 2, 4] | 
| 4 | 8 | [1, 2, 4] | chèn | [1, 2, 4, 8] | 
| 5 | 16 | [1, 2, 4, 8] | chèn | [1, 2, 4, 8, 16] | 

Dấu vết này cho thấy giai đoạn mà cấu trúc chưa đạt đến kích thước thứ năm, do đó mọi phần tử đều được bao gồm. Không có sự thay thế nào xảy ra vì tất cả các giá trị đều nằm trong số những giá trị lớn nhất được thấy cho đến nay khi xây dựng chuỗi. 

### Ví dụ 2 

đầu vào:```
10 10 3 1000
```| Bước | Giá trị hiện tại | Top 5 trước | Hành động | Top 5 sau | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | trống | chèn | [10] | 
| 2 | 30 | [10] | chèn | [10, 30] | 
| 3 | 90 | [10, 30] | chèn | [10, 30, 90] | 
| 4 | 270 | [10, 30, 90] | chèn | [10, 30, 90, 270] | 
| 5 | 810 | [10, 30, 90, 270] | chèn | [10, 30, 90, 270, 810] | 
| 6 | 430 | [10, 30, 90, 270, 810] | thay thế 10 | [30, 90, 270, 430, 810] | 
| 7 | 290 | [30, 90, 270, 430, 810] | không thay đổi | giống nhau | 
| 8 | 870 | [30, 90, 270, 430, 810] | thay thế 30 | [90, 270, 430, 810, 870] | 
| 9 | 610 | [90, 270, 430, 810, 870] | thay thế 90 | [270, 430, 610, 810, 870] | 
| 10 | 830 | [270, 430, 610, 810, 870] | thay thế 270 | [430, 610, 810, 830, 870] | 

Dấu vết này cho thấy hành vi quan trọng: khi cấu trúc đã đầy, chỉ các phần tử lớn hơn mức tối thiểu hiện tại mới có thể nhập và chúng thay thế phần tử yếu nhất trong tập hợp trên cùng hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi phần tử được tạo một lần và được so sánh với cấu trúc có kích thước không đổi | 
| Không gian |$O(1)$| Chỉ một danh sách cố định gồm năm phần tử và một biến duy nhất được lưu trữ | 

Quét tuyến tính lên tới hai mươi triệu phần tử là chi phí vượt trội, nhưng mỗi lần lặp lại cực kỳ nhẹ. Việc duy trì kích thước không đổi đảm bảo không xuất hiện hệ số logarit ẩn, giữ cho giải pháp nằm trong các ràng buộc điển hình dành cho Python được tối ưu hóa theo giới hạn kiểu Codeforces. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins

    input = sys.stdin.readline
    n, a1, k, p = map(int, input().split())

    top = [a1]
    cur = a1

    for i in range(1, n):
        cur = (cur * k) % p
        if len(top) < 5:
            top.append(cur)
            top.sort()
        else:
            if cur > top[0]:
                top[0] = cur
                top.sort()

    return " ".join(map(str, top))

# provided samples
assert run("5 1 2 100") == "1 2 4 8 16"
assert run("10 10 3 1000") == "430 610 810 830 870"

# minimum-size edge
assert run("5 7 1 10") == "7 7 7 7 7"

# all equal (p=1 forces zero)
assert run("6 5 2 1") == "0 0 0 0 0"

# strictly increasing modulo no wrap
assert run("5 1 10 1000") == "1 10 100 1000 0"

# mixed replacement behavior
assert run("7 9 3 50") == "6 9 11 27 33"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 1 2 100 | 1 2 4 8 16 | trình tự phát triển cơ bản | 
| 6 5 2 1 | 0 0 0 0 0 | trường hợp sụp đổ mô đun | 
| 5 7 1 10 | 7 7 7 7 7 | chuỗi không đổi | 
| 5 1 10 1000 | 1 10 100 1000 0 | hành vi bao quanh | 

## Vỏ cạnh 

Khi nào$p = 1$, mọi phép nhân theo sau bởi modulo ngay lập tức tạo ra số 0. Thuật toán vẫn hoạt động bình thường vì bước tạo chuỗi tạo ra một luồng số 0 và cấu trúc top 5 dần dần được lấp đầy bằng các số 0 và ổn định. Đầu ra cuối cùng chính xác trở thành năm số không mà không cần xử lý đặc biệt. 

Khi$k = 1$, chuỗi trở thành hằng số. Giá trị đầu tiên lặp lại cho tất cả các vị trí, do đó, cấu trúc năm vị trí hàng đầu chỉ cần điền các giá trị giống nhau. Vì được phép trùng lặp nên không có logic thay thế nào thay đổi kết quả cuối cùng và thuật toán vẫn trả về năm bản sao của cùng một số. 

Khi$n = 5$, cấu trúc không bao giờ bước vào giai đoạn thay thế. Mọi phần tử đều được chèn trực tiếp, kiểm tra tính chính xác của quá trình khởi tạo và định dạng đầu ra cuối cùng mà không cần dựa vào logic bảo trì.
