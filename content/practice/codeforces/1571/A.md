---
title: "CF 1571A - Trình tự so sánh"
description: "Chúng ta được cung cấp một chuỗi các dấu hiệu quan hệ giữa các phần tử liên tiếp của một mảng và chúng ta được yêu cầu xác định xem liệu chúng ta có thể suy ra duy nhất mối quan hệ giữa phần tử đầu tiên và phần tử cuối cùng hay không."
date: "2026-06-10T11:20:19+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 1571
codeforces_index: "A"
codeforces_contest_name: "Kotlin Heroes: Episode 8"
rating: 800
weight: 1571
solve_time_s: 91
verified: true
draft: false
---

[CF 1571A - Trình tự so sánh](https://codeforces.com/problemset/problem/1571/A) 

**Đánh giá:** 800 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các dấu hiệu quan hệ giữa các phần tử liên tiếp của một mảng và chúng ta được yêu cầu xác định xem liệu chúng ta có thể suy ra duy nhất mối quan hệ giữa phần tử đầu tiên và phần tử cuối cùng hay không. Cụ thể hơn, chúng ta không biết chính mảng đó, nhưng chúng ta biết đối với mỗi cặp liền kề phần tử đầu tiên nhỏ hơn, bằng hay lớn hơn phần tử thứ hai. Nhiệm vụ là quyết định xem những so sánh này có đủ để xác định xem phần tử đầu tiên nhỏ hơn, bằng hoặc lớn hơn phần tử cuối cùng hay không. Nếu có sự mơ hồ, chúng ta nên báo cáo bằng dấu chấm hỏi. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm là một chuỗi có độ dài tối đa 100 ký tự biểu thị các so sánh theo cặp này. Có nhiều nhất 500 trường hợp thử nghiệm, do đó, ngay cả giải pháp lặp qua chuỗi cho mọi trường hợp thử nghiệm cũng khả thi, vì tổng số thao tác nhiều nhất là 500 × 100 = 50.000, trong giới hạn thời gian 2 giây. Điều này loại trừ mọi lo ngại về việc cần cấu trúc dữ liệu phức tạp hơn hoặc thuật toán nâng cao; thách thức nằm ở việc hiểu được sự lan truyền của so sánh chứ không phải ở hiệu quả. 

Sự tinh tế nằm ở sự mơ hồ do dấu bằng gây ra. Ví dụ: một chuỗi như`<=<`chừa chỗ cho nhiều chuỗi số nhất quán. Người ta có thể tưởng tượng`[1, 2, 2, 1]`hoặc`[2, 2, 3, 1]`. Trong những trường hợp như vậy, phần tử đầu tiên và phần tử cuối cùng không thể được so sánh một cách duy nhất. Một trường hợp cạnh khác là một chuỗi chỉ có các dấu bằng, chẳng hạn như`===`, ngụ ý trực tiếp rằng tất cả các phần tử đều bằng nhau và do đó phần tử đầu tiên và phần tử cuối cùng cũng bằng nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là thử xây dựng lại các chuỗi số phù hợp với các mối quan hệ đã cho và sau đó kiểm tra xem sự so sánh giữa phần tử đầu tiên và phần tử cuối cùng có khác nhau hay không. Điều này có thể được thực hiện bằng cách thử tất cả các phép gán số nguyên có thể, nhưng ngay cả với độ dài 100, điều này sẽ bùng nổ về mặt tổ hợp. Về mặt khái niệm, nó hoạt động vì nếu bất kỳ cặp chuỗi nào tạo ra kết quả khác nhau cho phần tử đầu tiên và phần tử cuối cùng thì sự so sánh sẽ không rõ ràng. Vấn đề với phương pháp bạo lực là chúng ta không cần các giá trị thực tế, chỉ cần những thay đổi tương đối, vì vậy việc liệt kê các số là quá mức cần thiết. 

Cách tiếp cận tối ưu nhận thấy rằng các so sánh duy nhất quan trọng là các dấu hiệu không bằng nhau ngoài cùng bên trái và ngoài cùng bên phải. Nếu dấu không bằng đầu tiên là nhỏ hơn thì phần tử đầu tiên có khả năng nhỏ hơn phần tử tiếp theo. Tương tự, nếu dấu không bằng cuối cùng lớn hơn thì nó biểu thị mức giảm về cuối. Nếu chuỗi chỉ chứa`<`, chỉ một`>`hoặc sự kết hợp không có hướng xung đột, chúng ta có thể xác định trực tiếp mối quan hệ của phần tử đầu tiên và phần tử cuối cùng. Sự mơ hồ phát sinh chính xác khi chuỗi chứa cả hai`<`Và`>`dấu hiệu cách nhau bằng 0 hoặc nhiều dấu bằng. Bất kỳ dấu bằng nào ở giữa đều không ảnh hưởng đến thứ tự, do đó vấn đề giảm xuống còn việc kiểm tra sự hiện diện của cả hai`<`Và`>`trong chuỗi. Nếu cả hai đều tồn tại thì câu trả lời là`?`. Ngược lại, câu trả lời là có một hướng duy nhất, hoặc`=`nếu chỉ tồn tại dấu bằng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp kiểm thử, hãy kiểm tra chuỗi so sánh. Đếm xem có ít nhất một`<`và ít nhất một`>`trong chuỗi. Dấu bằng bị bỏ qua vì chúng không thay đổi thứ tự tương đối và có thể được hợp nhất theo một trong hai hướng. 
2. Nếu cả hai`<`Và`>`có mặt, trở về`?`. Điều này thể hiện sự mơ hồ: tồn tại các chuỗi nhất quán với chuỗi làm cho phần tử đầu tiên vừa nhỏ hơn vừa lớn hơn phần tử cuối cùng. 
3. Giá như`<`có dấu hiệu, hãy quay lại`<`vì tất cả các chuyển đổi đều biểu thị thứ tự không giảm dẫn đến phần tử cuối cùng ít nhất phải lớn bằng phần tử đầu tiên. Tương tự, nếu chỉ`>`có dấu hiệu, hãy quay lại`>`. 
4. Nếu chỉ có`=`dấu hiệu, trở lại`=`vì tất cả các phần tử đều bằng nhau. 

Tại sao nó hoạt động: Thuộc tính chính là chỉ những thay đổi hướng ngoài cùng mới xác định thứ tự có thể có của phần tử đầu tiên và cuối cùng. Dấu bằng không gây ra sự mơ hồ và sự hiện diện của cả hai`<`Và`>`bất kỳ vị trí nào trong chuỗi đều cho phép xây dựng nhiều chuỗi nhất quán trong đó phần tử đầu tiên và phần tử cuối cùng có thể nhỏ hơn hoặc lớn hơn nhau. Bất biến này đảm bảo tính chính xác mà không cần xây dựng lại các giá trị một cách rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()
    has_less = '<' in s
    has_greater = '>' in s
    if has_less and has_greater:
        print('?')
    elif has_less:
        print('<')
    elif has_greater:
        print('>')
    else:
        print('=')
```Giải pháp đọc số lượng trường hợp thử nghiệm, sau đó lặp lại từng chuỗi so sánh. Quyết định quan trọng được đưa ra bằng cách phát hiện sự hiện diện của`<`Và`>`trong chuỗi. Điều này tránh mọi nỗ lực không cần thiết để gán số thực tế cho các vị trí mảng. Xử lý chuỗi bằng`strip()`đảm bảo rằng các ký tự dòng mới ở cuối không ảnh hưởng đến việc kiểm tra ngăn chặn. Thứ tự của các kiểm tra có điều kiện cũng rất quan trọng: trường hợp mơ hồ được kiểm tra trước tiên, tiếp theo là các trường hợp một hướng và cuối cùng là trường hợp hoàn toàn bằng nhau. 

## Ví dụ đã hoạt động 

### Mẫu 1:`>>>`| Bước | has_less | has_Greater | Đầu ra | 
| --- | --- | --- | --- | 
| '>>>' | Sai | Đúng | '>' | 

Tất cả các dấu hiệu đều`>`, do đó phần tử đầu tiên lớn hơn phần tử cuối cùng. 

### Mẫu 2:`<><=<`| Bước | has_less | has_Greater | Đầu ra | 
| --- | --- | --- | --- | 
| '<><=<' | Đúng | Đúng | '?' | 

Cả hai`<`Và`>`tồn tại, làm cho việc so sánh từ đầu đến cuối trở nên mơ hồ. 

Những dấu vết này cho thấy thuật toán xác định chính xác sự hiện diện của cả hai hướng là nguồn gốc của sự mơ hồ và các chuỗi chỉ có một hướng hoặc tất cả các dấu bằng đều dẫn đến một quyết định duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét từng chuỗi so sánh một lần để tìm`<`Và`>`trong mỗi trường hợp thử nghiệm. Tổng số thao tác tỷ lệ thuận với tổng độ dài chuỗi. | 
| Không gian | O(1) | Chỉ có hai cờ boolean được sử dụng cho mỗi trường hợp thử nghiệm và không cần thêm mảng hoặc cấu trúc dữ liệu. | 

Cho n ≤ 100 và t ≤ 500, tổng công việc tối đa là 50.000 thao tác, không đáng kể so với giới hạn 2 giây. Việc sử dụng bộ nhớ cũng ở mức tối thiểu nên giải pháp dễ dàng phù hợp với những hạn chế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    out = []
    for _ in range(t):
        s = input().strip()
        has_less = '<' in s
        has_greater = '>' in s
        if has_less and has_greater:
            out.append('?')
        elif has_less:
            out.append('<')
        elif has_greater:
            out.append('>')
        else:
            out.append('=')
    return '\n'.join(out)

# provided samples
assert run("4\n>>>\n<><=<\n=\n<<==") == ">\n?\n=\n<", "sample 1"

# custom cases
assert run("1\n<") == "<", "single less"
assert run("1\n>") == ">", "single greater"
assert run("1\n=") == "=", "single equal"
assert run("1\n<<<<<") == "<", "all less"
assert run("1\n>>>>>") == ">", "all greater"
assert run("1\n<><><>") == "?", "alternating ambiguity"
assert run("1\n=====") == "=", "all equal signs"
assert run("1\n<==>") == "?", "equal signs in between both directions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`<`|`<`| So sánh đơn, trường hợp đơn giản | 
|`>`|`>`| So sánh đơn, trường hợp đơn giản | 
|`=`|`=`| So sánh đơn bằng nhau | 
|`<<<<<`|`<`| Chỉ nhiều dấu hiệu nhỏ hơn | 
|`>>>>>`|`>`| Chỉ có nhiều dấu lớn hơn | 
|`<><><>`|`?`| Hướng đi xen kẽ, mơ hồ | 
|`=====`|`=`| Tất cả các dấu bằng | 
|`<==>`|`?`| Biển báo bằng nhau ngăn cách các hướng xung đột | 

## Vỏ cạnh 

Xem xét đầu vào`<==>`. Bộ thuật toán`has_less = True`Và`has_greater = True`, vì vậy đầu ra là`?`. Điều này đúng vì chúng ta có thể xây dựng các chuỗi như`[1, 2, 2, 0]`làm`a_1 > a_n`, hoặc`[0, 1, 1, 2]`làm`a_1 < a_n`. Các dấu bằng không ngăn được sự mơ hồ. Điều này xác nhận rằng dấu bằng được bỏ qua một cách hợp lý khi xác định xung đột. 

Vì`====`,`has_less = False`Và`has_greater = False`, dẫn đến đầu ra`=`. Điều này xử lý chính xác trường hợp cạnh trong đó tất cả các phần tử đều bằng nhau, bao gồm các mảng có độ dài tối thiểu 2.
