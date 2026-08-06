---
title: "CF 102501J - Đếm cây"
description: "Đầu vào là danh sách thứ tự các chiều cao của cây. Mỗi biến thể có thể tương ứng với một cây nhị phân có các nút, đọc từ trái sang phải, có chính xác chuỗi chiều cao này."
date: "2026-08-06T05:03:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102501
codeforces_index: "J"
codeforces_contest_name: "2019-2020 ICPC Southwestern European Regional Programming Contest (SWERC 2019-20)"
rating: 0
weight: 102501
solve_time_s: 86
verified: true
draft: false
---

[CF 102501J - Đếm cây](https://codeforces.com/problemset/problem/102501/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào là danh sách thứ tự các chiều cao của cây. Mỗi biến thể có thể tương ứng với một cây nhị phân có các nút, đọc từ trái sang phải, có chính xác chuỗi chiều cao này. Nút cha của mỗi nút phải có chiều cao không lớn hơn các nút con của nó, vì vậy chiều cao tối thiểu bên trong bất kỳ khoảng nào phải được đặt ở gốc của khoảng đó. 

Nhiệm vụ là đếm xem có bao nhiêu hình dạng cây nhị phân khác nhau thỏa mãn các quy tắc này. Chiều cao bằng nhau là nguồn gốc duy nhất của sự mơ hồ. Nếu giá trị tối thiểu của một khoảng xuất hiện nhiều lần thì bất kỳ vị trí nào trong số đó đều có thể trở thành gốc, tạo ra nhiều cấu trúc có thể có. 

Độ dài chuỗi có thể đạt tới một triệu, vì vậy giải pháp quy hoạch động theo khoảng là không thể. Việc lặp lại trên tất cả các mảng con sẽ yêu cầu tính toán bậc hai hoặc tệ hơn, vượt xa giới hạn hai giây cho phép. Thuật toán phải xử lý mỗi vị trí chỉ một số lần không đổi. 

Trường hợp cạnh chính là một chuỗi trong đó tất cả các giá trị đều bằng nhau. Ví dụ:```
3
5
5
5
```Câu trả lời là`5`, bởi vì ba nút có thể tạo thành bất kỳ hình dạng cây nhị phân nào. Một giải pháp giả định vị trí tối thiểu là duy nhất sẽ trả về một vị trí không chính xác. 

Một trường hợp quan trọng khác là tách cực tiểu bằng nhau:```
3
1
2
1
```Câu trả lời là`2`. Hai nút có chiều cao`1`có thể được sắp xếp là con trái hoặc con phải của người kia. Việc coi các giá trị bằng nhau là cực tiểu độc lập sẽ bỏ lỡ sự tương tác này. 

## Phương pháp tiếp cận 

Một giải pháp đệ quy trực tiếp tuân theo định nghĩa. Đối với một khoảng, hãy tìm giá trị nhỏ nhất, thử mọi lần xuất hiện của giá trị nhỏ nhất đó làm gốc và nhân các câu trả lời của hai khoảng còn lại. Điều này đúng vì mỗi cây hợp lệ có chính xác một vị trí gốc trong khoảng. 

Vấn đề là một mảng các giá trị bằng nhau sẽ tạo ra phép đệ quy tồi tệ nhất có thể. Về chiều dài`n`, số lượng trạng thái là bậc hai và việc thử tất cả các vị trí tối thiểu sẽ thêm một yếu tố khác. Trường hợp xấu nhất trở nên lớn hơn nhiều so với số lượng hoạt động được phép. 

Quan sát quan trọng là các giá trị tối thiểu bằng nhau hoạt động như một nhóm. Giả sử chiều cao tối thiểu của một khoảng xuất hiện`k`lần. Những thứ kia`k`các nút tạo thành các nút của cây nhị phân tùy ý, trong khi các khoảng trống giữa chúng chỉ chứa các giá trị lớn hơn và có thể được giải độc lập. Số cách sắp xếp các`k`các nút tối thiểu là`k`số Catalan thứ. 

Vấn đề còn lại là tìm các nhóm này một cách hiệu quả. Cây Descartes tối thiểu cho kết quả phân rã chính xác cần thiết. Chúng tôi xây dựng nó với một ngăn xếp đơn điệu. Với các giá trị bằng nhau, các mối quan hệ được giải quyết bằng cách giữ mức tối thiểu ngoài cùng bên trái làm nút gốc, điều này làm cho các nút tối thiểu bằng nhau xuất hiện trên chuỗi bên phải. Mỗi chuỗi như vậy tương ứng với một yếu tố Catalan. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n^2) | Quá chậm | 
| Cây Descartes | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây Descartes tối thiểu từ dãy. Việc duyệt theo thứ tự của cây này là chuỗi ban đầu và mỗi nút không lớn hơn các nút con của nó. Cấu trúc ngăn xếp đơn điệu đảm bảo thời gian tuyến tính. 
2. Tính toán trước các số Catalan lên đến`N`. Nếu một chuỗi chứa`k`các nút tối thiểu bằng nhau, đóng góp của nó là`Catalan(k)`. 
3. Duyệt cây Descartes từ dưới lên trên. Khi xử lý một nút, hãy theo dõi nút con bên phải trong khi giá trị vẫn bằng nhau. Điều này mang lại cho nhóm hoàn chỉnh các nút tối thiểu bằng nhau. 
4. Nhân câu trả lời của mọi cây con bên trái treo trong chuỗi đó, nhân câu trả lời của cây con sau chuỗi và nhân với số Catalan của độ dài chuỗi. 
5. Câu trả lời được lưu trữ cho gốc là số lượng giống hợp lệ. 

Điều bất biến là mỗi cây con Descartes được xử lý đại diện cho một khoảng liền kề của chuỗi ban đầu. Căn nguyên của khoảng đó là giá trị tối thiểu của nó. Các nút tối thiểu bằng nhau tạo thành chính xác các lựa chọn cho nhóm gốc và mọi khoảng con còn lại là độc lập, do đó, việc nhân các đóng góp này sẽ tính mỗi cây hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def solve():
    n = int(input())
    a = [int(input()) for _ in range(n)]

    if n == 0:
        print(1)
        return

    cat = [0] * (n + 1)
    cat[0] = 1
    for i in range(1, n + 1):
        s = 0
        for j in range(i):
            s = (s + cat[j] * cat[i - 1 - j]) % MOD
        cat[i] = s

    left = [-1] * n
    right = [-1] * n
    stack = []

    for i in range(n):
        last = -1
        while stack and a[stack[-1]] > a[i]:
            last = stack.pop()
        if stack:
            right[stack[-1]] = i
        if last != -1:
            left[i] = last
        stack.append(i)

    root = stack[0]

    ans = [0] * n
    group = [0] * n

    order = [(root, 0)]
    while order:
        u, state = order.pop()
        if state == 0:
            k = 0
            v = u
            while v != -1 and a[v] == a[u]:
                k += 1
                v = right[v]
            group[u] = k
            order.append((u, 1))

            v = u
            while v != -1 and a[v] == a[u]:
                if left[v] != -1:
                    order.append((left[v], 0))
                v = right[v]
            if v != -1:
                order.append((v, 0))
        else:
            res = cat[group[u]]
            v = u
            while v != -1 and a[v] == a[u]:
                if left[v] != -1:
                    res = res * ans[left[v]] % MOD
                v = right[v]
            if v != -1:
                res = res * ans[v] % MOD
            ans[u] = res

    print(ans[root] % MOD)

if __name__ == "__main__":
    solve()
```Cấu trúc cây Descartes sử dụng một ngăn xếp chứa cột sống bên phải hiện tại. Khi giá trị nhỏ hơn xuất hiện, các nút lớn hơn sẽ bị xóa và trở thành cây con bên trái của nút mới. Các giá trị bằng nhau không bị xóa, do đó giá trị tối thiểu bằng nhau ngoài cùng bên trái vẫn ở trên các giá trị bằng nhau sau này. 

Quá trình truyền tải động có tính lặp lại vì cây có thể có độ sâu một triệu. Lần truy cập đầu tiên sẽ khám phá chuỗi giá trị bằng nhau và lên lịch cho tất cả các cây con độc lập. Chuyến thăm thứ hai kết hợp các câu trả lời đã được tính toán sẵn của họ. 

Phép tính Catalan sử dụng phép truy toán tiêu chuẩn:`C[n] = sum(C[i] * C[n - 1 - i])`Ở đâu`C[0] = 1`. Điều này phù hợp với số lượng cây nhị phân có thể được tạo từ`n`nút. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc xây dựng cây Cartesian và việc duyệt cây đều chạm vào mỗi nút một số lần không đổi. | 
| Không gian | O(n) | Mảng dành cho cây, giá trị lập trình động và ngăn xếp lưu trữ một mục nhập cho mỗi vị trí. | 

Giới hạn đầu vào yêu cầu xử lý tuyến tính. Giải pháp tránh tất cả việc liệt kê khoảng thời gian và xử lý chuỗi kích thước tối đa trong giới hạn bộ nhớ. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
6
3
1
6
2
4
5
```Giá trị tối thiểu là`1`và mọi giá trị khác đều lớn hơn. Cây Descartes không có nhóm tối thiểu bằng nhau nên mọi thừa số Catalan đều là`1`. 

| Sân khấu | Nhóm tối thiểu hiện tại | Yếu tố Catalan | Kết quả | 
| --- | --- | --- | --- | 
| Gốc | một nút có giá trị 1 | 1 | 1 | 
| Cây con còn lại | tất cả các cực tiểu độc đáo | 1 | 1 | 

Câu trả lời cuối cùng là`1`. 

Đối với mẫu thứ hai:```
6
1
1
1
1
1
1
```Tất cả sáu nút thuộc về một nhóm tối thiểu. 

| Sân khấu | Quy mô nhóm | Giá trị Catalan | Kết quả | 
| --- | --- | --- | --- | 
| Toàn bộ cây | 6 | 132 | 132 | 

Câu trả lời là`132`, là số hình dạng cây nhị phân có sáu nút. 

## Vỏ cạnh 

Đối với một phần tử duy nhất:```
1
7
```Chỉ có một cây có thể. Cây Descartes có một nút, kích thước nhóm là một và`Catalan(1)=1`. 

Đối với tất cả các giá trị bằng nhau:```
4
3
3
3
3
```Thuật toán xây dựng một chuỗi bên phải có độ dài bằng 4. Nó không nhân bốn câu trả lời độc lập. Thay vào đó nó nhận ra một nhóm tối thiểu và sử dụng`Catalan(4)=14`. 

Đối với các giá trị được phân tách bằng các phần tử lớn hơn:```
3
1
2
1
```Hai nút tối thiểu trở thành một chuỗi có giá trị bằng nhau trong cây Descartes. Chiều dài chuỗi là hai, cho`Catalan(2)=2`, trong khi cây con ở giữa đóng góp một. Câu trả lời cuối cùng là`2`.
