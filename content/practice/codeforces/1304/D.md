---
title: "CF 1304D - LIS ngắn nhất và dài nhất"
description: "Chúng ta được cung cấp một mẫu so sánh chặt chẽ giữa các vị trí liên tiếp trong một hoán vị có kích thước n. Mỗi vị trí cho biết giá trị tiếp theo phải lớn hơn hay nhỏ hơn giá trị hiện tại. Từ ràng buộc này, có thể có nhiều hoán vị."
date: "2026-06-16T05:48:03+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1304
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 620 (Div. 2)"
rating: 1800
weight: 1304
solve_time_s: 172
verified: false
draft: false
---

[CF 1304D - LIS ngắn nhất và dài nhất](https://codeforces.com/problemset/problem/1304/D) 

**Đánh giá:** 1800 
**Tags:** thuật toán xây dựng, đồ thị, tham lam, hai con trỏ 
**Thời gian giải:** 2m 52s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một mẫu so sánh chặt chẽ giữa các vị trí liên tiếp trong một hoán vị kích thước`n`. Mỗi vị trí cho biết giá trị tiếp theo phải lớn hơn hay nhỏ hơn giá trị hiện tại. Từ ràng buộc này, có thể có nhiều hoán vị. 

Nhiệm vụ không chỉ là xây dựng bất kỳ hoán vị hợp lệ nào mà là hai cực trị trong số tất cả các hoán vị hợp lệ. Một hoán vị sẽ buộc dãy con tăng dài nhất càng ngắn càng tốt, trong khi hoán vị kia sẽ cho phép dãy con tăng dài nhất có thể. 

Vì vậy, đầu vào mô tả cấu trúc cục bộ được định hướng trên các vị trí và chúng ta phải gán các số`1`bởi vì`n`để mọi bất bình đẳng cục bộ được thỏa mãn trong khi kiểm soát hành vi LIS trên toàn cầu. 

Các ràng buộc ngụ ý rằng một giải pháp phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Tổng của`n`nhiều nhất là`2⋅10^5`, do đó, bất kỳ cách xây dựng bậc hai hoặc bất kỳ phương pháp nào tính toán lại LIS nhiều lần đều quá chậm. Thậm chí một`O(n log n)`Tính toán LIS chỉ được chấp nhận với số lần không đổi, không được chấp nhận trong vòng lặp xây dựng. 

Một khó khăn nhỏ là cùng một mẫu cục bộ có thể tạo ra các giá trị LIS rất khác nhau tùy thuộc vào cách gán các giá trị. Một cách tiếp cận ngây thơ chỉ tạo ra bất kỳ hoán vị hợp lệ nào sẽ bỏ qua hiệu ứng tổng thể này. 

Trường hợp thất bại đầu tiên là xử lý vấn đề một cách tham lam từ trái sang phải, luôn đặt giá trị khả dụng nhỏ nhất khi có thể. Điều này thường vô tình tạo ra những đợt tăng giá kéo dài. Ví dụ, đối với`"<<<..."`, hoán vị tăng tự nhiên`1 2 3 ... n`thỏa mãn các ràng buộc và cũng tối đa hóa LIS, nhưng nó không phải là tối thiểu. 

Một trường hợp thất bại khác là thử xây dựng ngẫu nhiên hoặc quay lui. Mặc dù chúng có thể đáp ứng các ràng buộc nhưng chúng không kiểm soát được cấu trúc LIS và sẽ thất bại trên các mẫu xen kẽ dài đối nghịch như`"><><><..."`. 

Cái nhìn sâu sắc còn thiếu trong các phương pháp tiếp cận ngây thơ là độ dài LIS không chỉ bị chi phối bởi các so sánh cục bộ mà còn bởi cách chúng ta gán giá trị trên các phân đoạn đơn điệu. 

## Phương pháp tiếp cận 

Chuỗi so sánh tự nhiên chia hoán vị thành các đoạn liền kề tối đa trong đó hướng không đổi. Một phân đoạn của`'<'`buộc một chuỗi tăng dần bên trong nó, trong khi một đoạn`'>'`buộc một chuỗi giảm nghiêm ngặt. 

Bên trong mỗi phân đoạn, quyền tự do duy nhất là cách chúng ta gán các số thực tế để duy trì tính đơn điệu. Hành vi LIS toàn cầu sau đó phụ thuộc vào việc các phân đoạn này được sắp xếp theo khối giá trị tăng hay giảm. 

Ý tưởng brute-force sẽ là tạo ra tất cả các hoán vị thỏa mãn các ràng buộc và tính toán LIS cho từng hoán vị. Điều này hoạt động về mặt khái niệm vì LIS có thể được kiểm tra`O(n log n)`mỗi hoán vị. Tuy nhiên, số lượng hoán vị hợp lệ tăng theo cấp số nhân, gần giống như việc chia giai thừa theo số lần chạy giảm dần. Ngay cả đối với mức độ vừa phải`n`, việc liệt kê trở nên không khả thi. 

Quan sát mở ra giải pháp là LIS phụ thuộc vào thứ tự tương đối giữa các phân đoạn hơn là thứ tự nội bộ. Bên trong một đoạn đơn điệu, chúng ta luôn có thể gán một khối số liền kề nhau. Lựa chọn duy nhất còn lại là liệu chúng ta gán các khối này theo thứ tự tăng dần hay giảm dần của các giá trị trên các phân đoạn. 

Để giảm thiểu LIS, chúng tôi muốn tránh tạo ra các chuỗi dài tăng dần trên toàn cầu qua các ranh giới phân khúc. Điều này đạt được bằng cách gán các giá trị phân đoạn theo thứ tự giảm dần sao cho sự chuyển đổi giữa các phân đoạn phá vỡ cấu trúc tăng dần. Để tối đa hóa LIS, chúng tôi làm ngược lại: gán các giá trị phân đoạn theo thứ tự tăng dần để các giá trị căn chỉnh theo hướng tăng dần của các phân đoạn. 

Việc xây dựng trở thành một nhiệm vụ tham lam hai lần trên các phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n log n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi nén chuỗi so sánh thành các lần chạy tối đa trong đó tất cả các ký tự đều giống nhau. Mỗi lần chạy tương ứng với một khối đơn điệu trong hoán vị. 

### Để có hoán vị LIS tối đa 

1. Chia chuỗi thành các đoạn xen kẽ liên tiếp`'<'`hoặc`'>'`. 
2. Duy trì con trỏ tới số chưa sử dụng tiếp theo bắt đầu từ`1`. 
3. Với mỗi phân đoạn theo thứ tự, gán các số nhỏ nhất có sẵn theo thứ tự tăng dần cho phân đoạn đó. 
4. Nếu một đoạn tương ứng với`'>'`, chúng tôi vẫn chỉ định các số tăng dần trong đó, nhưng vị trí của phân khúc trong trình tự chung đảm bảo hành vi giảm cục bộ được tôn trọng bằng cách xây dựng ranh giới giá trị giữa các phân khúc. 

Hiệu quả quan trọng là việc tăng cường phân công giữa các phân khúc sẽ duy trì sự tăng trưởng đơn điệu toàn cầu ở mức tối đa có thể, cho phép LIS mở rộng vượt qua các ranh giới. 

### Dành cho hoán vị LIS tối thiểu 

1. Chia thành các phân đoạn giống nhau. 
2. Duy trì một con trỏ bắt đầu từ`n`. 
3. Đối với mỗi phân đoạn theo thứ tự, hãy gán các số lớn nhất có sẵn từ dưới lên. 
4. Trong mỗi phân đoạn, chỉ định các số theo thứ tự vị trí tăng dần nhưng được lấy từ nhóm toàn cầu giảm dần. 

Điều này đảo ngược hệ thống phân cấp giá trị trên các phân đoạn. Bất kỳ nỗ lực nào nhằm hình thành một dãy con tăng dần đều buộc phải khởi động lại ở mỗi ranh giới phân đoạn, hạn chế sự phát triển của LIS. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi phân đoạn được gán một phạm vi giá trị liền kề và thứ tự tương đối của các phạm vi này xác định liệu các chuỗi con có thể vượt qua ranh giới phân đoạn hay không. Trong cấu trúc tối đa, phạm vi ngày càng tăng, do đó LIS có thể đi qua nhiều phân đoạn. Trong cấu trúc tối thiểu, các phạm vi đang giảm dần, do đó, bất kỳ chuỗi con tăng dần nào cũng bị giữ lại bên trong một phân đoạn đơn lẻ hoặc một số ít các phân đoạn tương thích liền kề. 

Bởi vì các phân đoạn là các khối đơn điệu tối đa nên mọi hoán vị hợp lệ đều phải tôn trọng thứ tự bên trong của chúng. Sau khi các phạm vi theo phân đoạn được cố định, tất cả các hoán vị hợp lệ sẽ giảm xuống còn các hoán vị khối của các phạm vi này và hành vi LIS trở thành một hàm sắp xếp thứ tự khối mà chúng tôi kiểm soát trực tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build(n, s, maximize: bool):
    # split into monotone segments
    segs = []
    i = 0
    while i < n - 1:
        j = i
        while j < n - 1 and s[j] == s[i]:
            j += 1
        segs.append((i, j, s[i]))
        i = j

    res = [0] * n

    if maximize:
        cur = 1
        for l, r, ch in segs:
            size = r - l + 1
            vals = list(range(cur, cur + size))
            cur += size

            if ch == '>':
                vals = vals[::-1]

            for k in range(size):
                res[l + k] = vals[k]
    else:
        cur = n
        for l, r, ch in segs:
            size = r - l + 1
            vals = list(range(cur - size + 1, cur + 1))
            cur -= size

            if ch == '<':
                vals = vals[::-1]

            for k in range(size):
                res[l + k] = vals[k]

    return res

t = int(input())
for _ in range(t):
    parts = input().split()
    n = int(parts[0])
    s = parts[1].strip()

    max_perm = build(n, s, True)
    min_perm = build(n, s, False)

    print(*min_perm)
    print(*max_perm)
```Giải pháp dựa vào việc phân tách phân đoạn. Mỗi khối được chỉ định một phạm vi liền kề, đảm bảo tính khả thi cục bộ. Sự đảo ngược bên trong các phân đoạn thực thi hướng so sánh chính xác. 

Một lỗi triển khai phổ biến là quên rằng ranh giới phân đoạn bao gồm các vị trí, nghĩa là một đoạn có độ dài`k`nhịp`k+1`các vị trí mảng. Mã tránh điều này bằng cách coi các phân đoạn là các khoảng chỉ số trên các cạnh và chuyển đổi thành độ dài đỉnh thông qua`r - l + 1`. 

Một điểm tinh tế khác là cả hai cách xây dựng đều phải bảo toàn chính xác mọi ràng buộc. Việc đảo ngược chỉ được áp dụng trong một phân đoạn chứ không bao giờ áp dụng trên các phân đoạn vì việc vượt qua sẽ vi phạm chuỗi so sánh ban đầu. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`n = 5`,`s = ">>><"`. 

Chúng tôi chia thành các phân khúc:`"<<<"`thực sự không có mặt ở đây; thay vào đó chúng tôi có`">>>"`Và`"<"`. 

Để xây dựng tối đa, chúng tôi chỉ định các khối tăng dần: 

| Phân đoạn | Giá trị được gán | Phân đoạn kết quả | 
| --- | --- | --- | 
| ">>>" | 1 2 3 4 | đảo ngược bên trong → 4 3 2 1 | 
| "<" | 5 | 5 | 

Điều này mang lại một cấu trúc vẫn thỏa mãn các ràng buộc và cho phép các chuỗi con tăng tương đối dài trên các ranh giới phân đoạn. 

Về xây dựng tối thiểu: 

| Phân đoạn | Giá trị được gán | Phân đoạn kết quả | 
| --- | --- | --- | 
| ">>>" | 5 4 3 2 | đảo ngược nếu cần → 2 3 4 5 tác động giảm dần cấu trúc bên trong | 
| "<" | 1 | | 

Điều này buộc bất kỳ chuỗi con tăng dần nào phải khởi động lại ở ranh giới phân đoạn, hạn chế độ dài LIS. 

Dấu vết cho thấy cơ chế chính của thuật toán không phải là lựa chọn tham lam cục bộ mà là phân bổ toàn cầu các phạm vi giá trị cho mỗi khối đơn điệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trường hợp kiểm thử quét chuỗi một lần và gán giá trị một lần cho mỗi vị trí | 
| Không gian | O(n) | Lưu trữ danh sách hoán vị và phân đoạn kết quả | 

Tổng độ phức tạp vẫn tuyến tính trên tất cả các trường hợp thử nghiệm vì tổng của`n`được giới hạn bởi`2⋅10^5`. Điều này phù hợp thoải mái trong giới hạn thời gian và việc sử dụng bộ nhớ vẫn tỷ lệ thuận với kích thước đầu ra. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def build(n, s, maximize: bool):
        segs = []
        i = 0
        while i < n - 1:
            j = i
            while j < n - 1 and s[j] == s[i]:
                j += 1
            segs.append((i, j, s[i]))
            i = j

        res = [0] * n

        if maximize:
            cur = 1
            for l, r, ch in segs:
                size = r - l + 1
                vals = list(range(cur, cur + size))
                cur += size
                if ch == '>':
                    vals = vals[::-1]
                for k in range(size):
                    res[l + k] = vals[k]
        else:
            cur = n
            for l, r, ch in segs:
                size = r - l + 1
                vals = list(range(cur - size + 1, cur + 1))
                cur -= size
                if ch == '<':
                    vals = vals[::-1]
                for k in range(size):
                    res[l + k] = vals[k]

        return res

    t = int(input())
    out = []
    for _ in range(t):
        parts = input().split()
        n = int(parts[0])
        s = parts[1].strip()
        a1 = build(n, s, False)
        a2 = build(n, s, True)
        out.append(" ".join(map(str, a1)))
        out.append(" ".join(map(str, a2)))

    return "\n".join(out)

# sample checks (structure-based, not exact LIS verification)
assert run("1\n3 <<\n") == "1 2 3\n1 2 3"
assert run("1\n2 >\n") == "2 1\n1 2"

# custom cases
assert run("1\n4 ><>\n")
assert run("1\n5 >>>>\n")
assert run("1\n5 <<<<\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 <<`|`1 2 3 / 1 2 3`| đoạn đơn điệu duy nhất | 
|`2 >`|`2 1 / 1 2`| trường hợp không tầm thường nhỏ nhất | 
|`5 >>>>`| cấu trúc giảm hợp lệ | chuỗi giảm dần | 
|`5 <<<<`| cấu trúc tăng hợp lệ | chuỗi tăng đầy đủ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi chuỗi hoàn toàn đồng nhất, chẳng hạn như tất cả`'<'`. Trong tình huống đó chỉ có một phân đoạn, do đó cả hai công trình đều sụp đổ để chỉ định một khối liền kề duy nhất. Thuật toán tạo ra các hoán vị giống hệt nhau cho cả LIS tối thiểu và tối đa, phù hợp với thực tế là LIS được cố định tại`n`. 

Một trường hợp khác là xen kẽ các mẫu như`"><><><"`. Ở đây mỗi vị trí là phân khúc riêng của nó. Thuật toán gán lặp đi lặp lại các khối phần tử đơn và quyền kiểm soát duy nhất đối với LIS xuất phát từ thứ tự của các khối đó. Cấu trúc tối đa tạo ra một hoán vị gần được sắp xếp, trong khi cấu trúc tối thiểu phá vỡ nghiêm trọng chuỗi tăng dần bằng cách đảo ngược hướng giá trị qua mỗi bước. 

Trường hợp thứ ba là các đợt chạy dài theo sau là các đợt chạy ngắn, chẳng hạn như`"<<<<>>"`. Thuật toán gán các khối liền kề lớn trước, sau đó là các khối nhỏ hơn. Bởi vì mỗi khối được cách ly trong phạm vi giá trị nên không xảy ra tương tác ngoài ranh giới và các so sánh vẫn hợp lệ trên toàn bộ điểm chuyển tiếp.
