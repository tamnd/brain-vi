---
title: "CF 1213A - Di chuyển chip"
description: "Chúng ta được cấp một số chip được đặt ở các vị trí số nguyên trên một dòng. Mục tiêu của chúng tôi là di chuyển tất cả các quân cờ sao cho chúng kết thúc trên một tọa độ chung duy nhất, sử dụng chuỗi di chuyển rẻ nhất có thể. Mỗi con chip có thể di chuyển theo hai cách khác nhau."
date: "2026-06-15T18:30:41+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1213
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 582 (Div. 3)"
rating: 900
weight: 1213
solve_time_s: 114
verified: true
draft: false
---

[CF 1213A - Di chuyển chip](https://codeforces.com/problemset/problem/1213/A) 

**Xếp hạng:** 900 
**Thẻ:** toán 
**Thời gian giải:** 1m 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số chip được đặt ở các vị trí số nguyên trên một dòng. Mục tiêu của chúng tôi là di chuyển tất cả các quân cờ sao cho chúng kết thúc trên một tọa độ chung duy nhất, sử dụng chuỗi di chuyển rẻ nhất có thể. 

Mỗi con chip có thể di chuyển theo hai cách khác nhau. Một con chip có thể nhảy sang trái hoặc sang phải 2 đơn vị mà không mất phí hoặc nó có thể di chuyển sang trái hoặc sang phải 1 đơn vị với chi phí là một đồng xu. Khó khăn chính là những bước di chuyển không tốn phí cho phép chúng ta thay đổi vị trí chẵn lẻ một cách tự do trong cùng một lớp chẵn lẻ, trong khi những thay đổi lẻ mới thực sự tạo ra chi phí. 

Nhiệm vụ là tính toán tổng số xu tối thiểu cần thiết để tất cả các chip có thể được di chuyển vào cùng một vị trí số nguyên. 

Các ràng buộc rất nhỏ, tối đa là 100 chip. Điều đó có nghĩa là bất kỳ giải pháp nào thử tất cả các vị trí mục tiêu ứng cử viên đều khả thi vì việc kiểm tra một mục tiêu duy nhất là tuyến tính theo n và thậm chí quét tất cả tọa độ trong phạm vi đầu vào vẫn có thể chấp nhận được trong O(n^2). Tuy nhiên, cấu trúc của các bước di chuyển gợi ý rằng nên tồn tại một sự đơn giản hóa về mặt toán học hơn và giải pháp tối ưu sẽ giảm vấn đề thành một thứ độc lập với các giá trị tọa độ lớn lên tới 10^9. 

Một cách tiếp cận đơn giản sẽ xem xét việc chọn mọi tọa độ mục tiêu có thể có giữa vị trí chip tối thiểu và tối đa và tính toán chi phí di chuyển tất cả các chip đến đó. Vấn đề không phải là tính chính xác mà là sự dư thừa, vì nhiều mục tiêu hoạt động giống hệt nhau do tính chẵn lẻ. 

Trường hợp khó nhận biết xuất hiện khi tất cả các chip đã ở cùng một vị trí. Trong trường hợp đó, câu trả lời là 0 và bất kỳ lý do nào dựa trên chi phí di chuyển đều phải bảo toàn điều này. Một trường hợp cạnh khác là khi tất cả các vị trí có cùng tính chẵn lẻ, ví dụ như tất cả các số chẵn. Trong trường hợp này, những bước đi không tốn phí có thể sắp xếp mọi thứ, dẫn đến câu trả lời bằng 0, mặc dù khoảng cách giữa các điểm có thể lớn. Một cách tiếp cận ngây thơ giả định không chính xác chi phí bằng khoảng cách sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Quan sát quan trọng là việc di chuyển theo 2 là miễn phí, vì vậy bất kỳ con chip nào cũng có thể di chuyển tự do giữa tất cả các vị trí có cùng số chẵn lẻ mà không phải trả bất kỳ khoản nào. Điều này có nghĩa là lần duy nhất chúng ta trả xu là khi chúng ta cần thay đổi số chẵn lẻ, vì một bước di chuyển ±1 sẽ làm lật số chẵn lẻ và tốn một xu. 

Điều này biến vấn đề thành vấn đề cân bằng chẵn lẻ hơn là vấn đề giảm thiểu khoảng cách. 

Đối với bất kỳ tọa độ mục tiêu T nào được chọn, một con chip ở vị trí x có hai khả năng. Nếu x và T có cùng số chẵn lẻ, chúng ta có thể di chuyển nó hoàn toàn bằng cách sử dụng ±2 bước, không trả xu nào. Nếu tính chẵn lẻ của chúng khác nhau, chúng ta phải trả ít nhất một xu để điều chỉnh tính chẵn lẻ và sau đó phần còn lại có thể được xử lý bằng các nước đi tự do ±2. Điều quan trọng là khi một con chip trả tiền cho một nước đi ±1, nó có thể căn chỉnh tính chẵn lẻ của nó với T một cách hiệu quả và hoàn thành phần còn lại miễn phí. 

Vì vậy, chi phí cho mục tiêu cố định T chỉ đơn giản là số lượng chip có tính chẵn lẻ khác với T. 

Chỉ có hai lựa chọn có ý nghĩa cho tính chẵn lẻ của T: chẵn hoặc lẻ. Nếu T là số chẵn thì mỗi chip vị trí chẵn có giá bằng 0 và mỗi chip vị trí lẻ có giá một xu. Nếu T là số lẻ thì vai trò sẽ đảo ngược. Do đó, đáp án là giá trị nhỏ nhất giữa số vị trí chẵn và số vị trí lẻ. 

Cách tiếp cận bạo lực sẽ thử mọi tọa độ mục tiêu có thể, tính toán sự không khớp chẵn lẻ cho tất cả các chip và lấy mức tối thiểu. Vì tính chẵn lẻ xác định đầy đủ chi phí nên điều này giảm xuống chỉ còn hai trường hợp thay vì tối đa 10^9 ứng viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tọa độ | O(n * phạm vi) | O(1) | Quá chậm/không cần thiết | 
| Đếm chẵn lẻ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đếm xem có bao nhiêu vị trí chip chẵn và bao nhiêu vị trí chip lẻ. Điều này trực tiếp nắm bắt số lượng chip sẽ phù hợp với mức chẵn lẻ mục tiêu đã chọn. 
2. Tính số xu cần thiết nếu chúng ta chọn vị trí mục tiêu chẵn. Trong trường hợp đó, mỗi chip vị trí lẻ phải trả chính xác một đồng xu để lật chẵn lẻ, do đó chi phí sẽ là số vị trí lẻ. 
3. Tính số xu cần thiết nếu chúng ta chọn vị trí mục tiêu lẻ. Một cách đối xứng, chi phí này bằng số vị trí chẵn. 
4. Trả về giá trị nhỏ nhất của hai giá trị. Điều này tương ứng với việc chọn chẵn lẻ mục tiêu để giảm thiểu số lần lật chẵn lẻ cần thiết. 

### Tại sao nó hoạt động 

Điều bất biến là sau tối đa một ±1 lần di chuyển trên mỗi chip, mọi chip có thể được căn chỉnh theo lớp chẵn lẻ mục tiêu và từ thời điểm đó, tất cả các chuyển động còn lại có thể được thực hiện bằng cách sử dụng ±2 bước tự do. Vì việc di chuyển ±2 duy trì tính chẵn lẻ và cho phép di chuyển không hạn chế trong một lớp chẵn lẻ nên khoảng cách không còn đóng góp vào chi phí nữa. Chi phí không thể giảm được duy nhất là liệu một con chip có bắt đầu ở lớp chẵn lẻ chính xác so với mục tiêu đã chọn hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
x = list(map(int, input().split()))

even = 0
odd = 0

for v in x:
    if v % 2 == 0:
        even += 1
    else:
        odd += 1

print(min(even, odd))
```Việc triển khai chỉ theo dõi số lượng chẵn lẻ. Vòng lặp tách các số thành các nhóm chẵn và lẻ, tương ứng trực tiếp với hai lựa chọn chẵn lẻ mục tiêu có thể có. Câu trả lời cuối cùng là kích thước thùng nhỏ hơn. 

Không cần sắp xếp hoặc nén tọa độ vì các vị trí tuyệt đối ngoài tính chẵn lẻ là không liên quan. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```| Bước | Tính chẵn | Số lẻ | Chi phí nếu thậm chí là mục tiêu | Chi phí nếu mục tiêu lẻ | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 3 | 3 | 0 | 0 | 

Nếu chúng ta chọn mục tiêu chẵn, cả ba chip phải lật chẵn lẻ, tốn 3 xu. Nếu chúng ta chọn mục tiêu lẻ, không có chip nào cần lật chẵn lẻ sau khi căn chỉnh, vì tất cả đều là số lẻ nên chi phí bằng 0. Lựa chọn tối ưu là số lẻ. 

### Ví dụ 2 

đầu vào:```
4
1 2 3 4
```| Bước | Tính chẵn | Số lẻ | Chi phí nếu thậm chí là mục tiêu | Chi phí nếu mục tiêu lẻ | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 2 | 2 | 2 | 2 | 2 | 

Cả hai số chẵn lẻ đều cân bằng, do đó lựa chọn nào cũng tạo ra chi phí như nhau. Câu trả lời là 2. 

Những ví dụ này cho thấy rằng chỉ có phân phối chẵn lẻ mới quan trọng chứ không phải khoảng cách hoặc thứ tự thực tế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chip được xử lý một lần để xác định tính chẵn lẻ | 
| Không gian | O(1) | Chỉ có hai quầy được duy trì | 

Các ràng buộc cho phép tối đa 100 chip, do đó quét tuyến tính có hiệu quả không đáng kể. Ngay cả với n lớn hơn nhiều, giải pháp này vẫn tối ưu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    x = list(map(int, sys.stdin.readline().split()))

    even = 0
    odd = 0
    for v in x:
        if v % 2 == 0:
            even += 1
        else:
            odd += 1

    return str(min(even, odd))

# provided sample
assert run("3\n1 2 3\n") == "1"

# all same parity (already optimal)
assert run("5\n2 4 6 8 10\n") == "0"

# all odd
assert run("4\n1 3 5 7\n") == "0"

# mixed balanced
assert run("4\n1 2 3 4\n") == "2"

# single element
assert run("1\n100\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả thậm chí | 0 | trường hợp căn chỉnh chi phí bằng 0 | 
| tất cả đều kỳ quặc | 0 | đối xứng với chữ chẵn | 
| hỗn hợp cân bằng | 2 | chia chẵn lẻ bằng nhau | 
| chip đơn | 0 | trường hợp cạnh tối thiểu | 

## Vỏ cạnh 

Khi tất cả các chip đã ở vị trí giống hệt nhau, chẳng hạn như`5, 5, 5`, cả số chẵn và số lẻ sẽ gộp lại thành một nhóm duy nhất và câu trả lời sẽ trở thành số 0 ngay lập tức. Thuật toán xử lý việc này một cách tự nhiên vì một trong các số chẵn lẻ bằng 0. 

Khi tất cả các chip có cùng tính chẵn lẻ nhưng có sự khác biệt lớn về tọa độ, chẳng hạn như`2, 100, 100000`, giải pháp vẫn trả về 0 vì căn chỉnh chẵn lẻ sẽ loại bỏ tất cả chi phí, bất kể khoảng cách. 

Khi phân phối được trộn lẫn, chẳng hạn như xen kẽ các vị trí chẵn và lẻ, mỗi chip sẽ góp phần vào chính xác một trong các nhóm chẵn lẻ. Thuật toán vẫn chọn chính xác nhóm nhỏ hơn, đảm bảo số lần lật chẵn lẻ tối thiểu.
