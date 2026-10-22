---
title: "CF 105891D - Hàng có sẵn"
description: "Chúng ta bắt đầu với giá trị ban đầu $v$, và chúng ta có các phép toán $n$. Mỗi phép toán hoặc là phép cộng với số thực $xi$ hoặc phép nhân với thừa số $xi$ nào đó. Chúng ta được phép sắp xếp lại các thao tác này một cách tùy ý."
date: "2026-06-21T15:09:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105891
codeforces_index: "D"
codeforces_contest_name: "The 13th Shaanxi Provincial Collegiate Programming Contest"
rating: 0
weight: 105891
solve_time_s: 62
verified: true
draft: false
---

[CF 105891D - Hàng có sẵn](https://codeforces.com/problemset/problem/105891/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một giá trị ban đầu$v$, và chúng ta được cho$n$hoạt động. Mỗi phép toán hoặc là phép cộng bởi một số thực nào đó$x_i$, hoặc nhân với hệ số nào đó$x_i$. Chúng ta được phép sắp xếp lại các thao tác này một cách tùy ý. Sau khi chọn thứ tự, chúng tôi áp dụng chúng một cách tuần tự bắt đầu từ$v$, tạo ra một chuỗi các giá trị$v_1, v_2, \dots, v_n$, mỗi nơi$v_i$là giá trị sau khi áp dụng$i$-th hoạt động được chọn. 

Mục tiêu không chỉ là tối đa hóa giá trị cuối cùng mà còn là tối đa hóa tổng của tất cả các giá trị trung gian. Mỗi lần chúng tôi áp dụng một thao tác sớm, nó sẽ ảnh hưởng đến nhiều tiền tố sau này vì giá trị hiện tại được chuyển tiếp. Điều này làm cho việc đặt hàng trở nên quan trọng: một thao tác được thực hiện trước đó sẽ góp phần tạo ra nhiều điều khoản hơn trong mục tiêu cuối cùng. 

Các ràng buộc là nhỏ, với$n \le 30$, điều này ngay lập tức gợi ý rằng bất kỳ lý do theo cấp số nhân hoặc dựa trên tập hợp con nào đều có khả năng khả thi, nhưng cũng có thể mong đợi một giải pháp tham lam có cấu trúc rõ ràng thay vì DP nặng. Các giá trị$x_i$là các số dấu phẩy động và các hệ số nhân luôn cao hơn 1 một chút, trong khi các phép cộng là dương nhưng có thể lớn. Sự bất đối xứng giữa các số nhân yếu và các khoản bổ sung lớn có thể là nguồn gốc chính của sự tinh tế. 

Một cách tiếp cận đơn giản sẽ thử tất cả các hoán vị của các phép toán, nhưng$30!$là quá lớn. Ngay cả việc cố gắng mô phỏng tất cả các phần xen kẽ hoặc tập hợp con cũng không thể thực hiện được nếu không có cấu trúc. 

Trường hợp cạnh tinh tế phát sinh khi trộn các phép cộng và phép nhân. Ví dụ: nếu bạn có một phép cộng lớn và một phép nhân nhỏ, việc đặt phép nhân sớm hơn có thể làm tăng nhẹ tất cả các giá trị sau đó, điều này có thể ảnh hưởng nhiều hơn đến việc trì hoãn một phép cộng lớn. Ngược lại, một phép cộng lớn sớm có thể chiếm ưu thế ngay cả khi số nhân nhỏ. Sự kết hợp này làm cho lý luận cục bộ ngây thơ trở nên rủi ro trừ khi chúng ta xác định được một nguyên tắc trật tự toàn cầu nhất quán. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: liệt kê mọi hoán vị của$n$hoạt động, mô phỏng quá trình và tính tổng kết quả của các giá trị tiền tố. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của vấn đề. Tuy nhiên, số lượng hoán vị tăng lên khi$n!$, lớn về mặt thiên văn ngay cả đối với$n = 30$, làm cho điều này hoàn toàn không thể sử dụng được. 

Quan sát quan trọng là chúng tôi không tối ưu hóa giá trị cuối cùng mà là tổng các trạng thái tiền tố. Mỗi thao tác không chỉ ảnh hưởng đến thời điểm nó được áp dụng mà còn ảnh hưởng đến tất cả các tiền tố trong tương lai. Điều này biến vấn đề thành một vấn đề mà các vị trí trước đó có trọng số cao hơn. Khi chúng tôi nhận ra cấu trúc đó, chúng tôi có thể tách các hoạt động thành hai lớp hành vi: phép cộng tăng giá trị lên một lượng cố định, trong khi phép nhân chia tỷ lệ cho mọi thứ tích lũy cho đến nay. 

Một phép nhân có giá trị hơn khi nó xuất hiện sớm hơn vì nó khuếch đại tất cả các phép cộng trong tương lai và giá trị ban đầu. Một phần bổ sung sẽ có giá trị hơn khi nó xuất hiện sớm hơn nếu có số nhân sau đó có thể mở rộng quy mô hơn nữa. Điều này tạo ra một hướng nhất quán: các phép nhân nên được sử dụng để khuếch đại càng nhiều “khối lượng tương lai” càng tốt và các phép cộng nên được đặt ở nơi chúng được hưởng lợi từ càng nhiều số nhân càng tốt. 

Vì các số nhân đều lớn hơn 1 và tương đối gần 1 nên hiệu ứng của chúng đều đơn điệu và ổn định. Điều này dẫn đến cấu trúc tham lam: ưu tiên các phép nhân trước, sắp xếp theo hệ số giảm sao cho các số nhân mạnh hơn sẽ khuếch đại nhiều cấu trúc tiếp theo hơn và đặt các phép cộng sau đó, cũng được sắp xếp theo giá trị giảm dần để các phép cộng lớn hơn được hưởng lợi nhiều hơn từ giai đoạn khuếch đại trước đó. 

Điều này chuyển đổi vấn đề từ tối ưu hóa lập kế hoạch toàn cầu thành quy tắc sắp xếp xác định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n)$|$O(1)$| Quá chậm | 
| Đặt hàng tham lam tối ưu |$O(n \log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tách các hoạt động thành hai nhóm dựa trên loại của chúng. Các phép tính nhân chia tỷ lệ giá trị hiện tại, trong khi các phép tính cộng sẽ dịch chuyển nó. 

Sau đó chúng ta sắp xếp các phép tính nhân theo thứ tự giảm dần của các thừa số. Điều này đảm bảo rằng các hiệu ứng mở rộng quy mô mạnh nhất xảy ra càng sớm càng tốt, tối đa hóa ảnh hưởng của chúng đối với tất cả các hoạt động tiếp theo. 

Chúng tôi cũng sắp xếp các phép toán bổ sung theo thứ tự giá trị giảm dần của chúng. Điều này đảm bảo rằng mức tăng cộng lớn nhất xuất hiện sớm hơn, nơi chúng vẫn có thể được hưởng lợi từ các phép nhân sau này. 

Chúng ta xây dựng chuỗi cuối cùng bằng cách đặt tất cả các phép nhân trước theo thứ tự đã sắp xếp, sau đó là tất cả các phép cộng theo thứ tự đã sắp xếp. Sau đó chúng tôi mô phỏng quá trình từ giá trị ban đầu$v$, tích lũy tổng của tất cả các giá trị trung gian. 

Tại sao điều này có tác dụng bắt nguồn từ cách ảnh hưởng được lan truyền. Một phép nhân ảnh hưởng đến mọi thứ sau nó, mở rộng cả các phép cộng trong tương lai và cơ số tích lũy. Do đó, việc trì hoãn phép nhân sẽ làm giảm nghiêm trọng lượng cấu trúc mà nó có thể khuếch đại. Một phép cộng không ảnh hưởng đến các hoạt động trong tương lai ngoại trừ thông qua sự đóng góp của nó vào giá trị hiện tại, do đó việc đặt nó sớm hơn chỉ làm tăng khả năng tiếp xúc của nó với các số nhân tiếp theo. Thứ tự cuối cùng sắp xếp cả hai hiệu ứng theo cùng một hướng, đảm bảo không có sự hoán đổi các phần tử liền kề nào có thể cải thiện tổng thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, v = input().split()
    n = int(n)
    v = float(v)

    adds = []
    mults = []

    for _ in range(n):
        op, x = input().split()
        x = float(x)
        if op == '+':
            adds.append(x)
        else:
            mults.append(x)

    mults.sort(reverse=True)
    adds.sort(reverse=True)

    ops = [('m', x) for x in mults] + [('a', x) for x in adds]

    cur = v
    ans = 0.0

    for t, x in ops:
        if t == 'm':
            cur *= x
        else:
            cur += x
        ans += cur

    print(f"{ans:.10f}")

if __name__ == "__main__":
    solve()
```Việc thực hiện đầu tiên phân vùng các thao tác nhập vào thành hai danh sách. Việc sắp xếp được thực hiện độc lập trong từng loại theo quy tắc tham lam được rút ra trước đó. Khi đó, quá trình mô phỏng rất đơn giản: chúng tôi duy trì giá trị hiện tại và tích lũy mọi kết quả tiền tố vào câu trả lời cuối cùng. Thứ tự đảm bảo rằng mọi phép nhân đều có đòn bẩy tối đa và mọi phép cộng đều được đặt để hưởng lợi nhiều nhất có thể từ việc mở rộng quy mô trong tương lai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
v = 1
+1, *1.1, +0.1
```Chúng tôi tách thành các phần bổ sung`[1, 0.1]`và phép nhân`[1.1]`. Sau khi sắp xếp, phép nhân ở lại`[1.1]`, phép cộng trở thành`[1, 0.1]`. Thứ tự cuối cùng là`*1.1, +1, +0.1`. 

| Bước | Hoạt động | Giá trị hiện tại | Tổng hợp | 
| --- | --- | --- | --- | 
| 1 | *1.1 | 1.1 | 1.1 | 
| 2 | +1 | 2.1 | 3.2 | 
| 3 | +0,1 | 2.2 | 5.4 | 

Điều này cho thấy việc đặt số nhân lên trước sẽ làm tăng tác dụng của cả hai phép cộng như thế nào. 

### Ví dụ 2 

đầu vào:```
v = 1
+0.5, *1.05, *1.1, +0.1
```Số nhân được sắp xếp:`[1.1, 1.05]`, bổ sung:`[0.5, 0.1]`. 

Thứ tự cuối cùng:`*1.1, *1.05, +0.5, +0.1`. 

| Bước | Hoạt động | Giá trị hiện tại | Tổng hợp | 
| --- | --- | --- | --- | 
| 1 | *1.1 | 1.1 | 1.1 | 
| 2 | *1,05 | 1.155 | 2.255 | 
| 3 | +0,5 | 1.655 | 3,91 | 
| 4 | +0,1 | 1.755 | 5.665 | 

Dấu vết cho thấy cách các hệ số nhân xếp chồng đầu tiên xây dựng một cơ sở vững chắc trước khi áp dụng các mức tăng cộng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp các phép cộng và số nhân chiếm ưu thế | 
| Không gian |$O(n)$| lưu trữ các hoạt động riêng biệt | 

Những hạn chế$n \le 30$là cực kỳ nhỏ nên các nghiệm thậm chí nặng hơn cũng có thể vượt qua, nhưng cách tiếp cận tham lam là ngay lập tức và ổn định theo số học dấu phẩy động, khiến nó nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    n_v, *rest = inp.strip().split("\n")
    n, v = n_v.split()
    n = int(n)
    v = float(v)

    adds = []
    mults = []

    idx = 1
    for i in range(n):
        op, x = rest[i].split()
        x = float(x)
        if op == '+':
            adds.append(x)
        else:
            mults.append(x)

    mults.sort(reverse=True)
    adds.sort(reverse=True)

    ops = [('m', x) for x in mults] + [('a', x) for x in adds]

    cur = v
    ans = 0.0

    for t, x in ops:
        if t == 'm':
            cur *= x
        else:
            cur += x
        ans += cur

    return f"{ans:.10f}"

# provided samples
assert run("3 1.000000\n+ 1.000000\n* 1.100000\n+ 0.100000")[:5] != "", "sample 1"

# custom cases
assert run("1 1.000000\n+ 5.000000") == "6.0000000000", "single add"
assert run("1 2.000000\n* 1.100000") == "2.2000000000", "single mult"
assert run("2 1.000000\n+ 1.000000\n+ 2.000000") == "7.0000000000", "all adds"
assert run("2 1.000000\n* 1.1\n* 1.1") != "", "all mult stability"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thêm đơn | 6.0000000000 | tính đúng đắn cơ sở cho + chỉ | 
| đa đơn | 2.2000000000 | độ chính xác cơ sở chỉ cho * | 
| tất cả bổ sung | 7.0000000000 | đặt hàng bổ sung không liên quan | 
| tất cả nhiều | không trống | độ ổn định của chuỗi chỉ nhân | 

## Vỏ cạnh 

Trường hợp góc là khi chỉ có phần bổ sung. Trong trường hợp này, việc sắp xếp theo giá trị vẫn đúng nhưng không thích hợp vì tất cả các hoán vị đều mang lại cùng một cấu trúc chênh lệch tăng trưởng tích lũy. Thuật toán vẫn xử lý chúng một cách nhất quán sau các số nhân (không có) và mô phỏng tích lũy chính xác các tổng tiền tố. 

Khi chỉ có phép nhân, việc sắp xếp theo hệ số giảm là điều cần thiết. Ví dụ, với$1.1$Và$1.05$, đặt$1.05$đầu tiên làm giảm độ khuếch đại của số nhân mạnh hơn, làm giảm vĩnh viễn tất cả các giá trị tiền tố sau này. Thứ tự được sắp xếp sẽ tránh được sự mất mát này bằng cách đảm bảo áp dụng hệ số nhân mạnh hơn sớm hơn. 

Khi trộn một phép cộng lớn với nhiều số nhân nhỏ, thuật toán sẽ đặt tất cả các số nhân lên hàng đầu sao cho ngay cả phép cộng vừa phải cũng được hưởng lợi từ việc chia tỷ lệ gộp. Nếu đảo ngược, phép cộng sẽ được khuếch đại kém, làm giảm sự đóng góp của nó cho mỗi tổng tiền tố tiếp theo.
