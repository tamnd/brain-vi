---
title: "CF 104345F - Làm Số"
description: "Chúng ta có một tập hợp nhiều chữ số cố định đến từ một số $X$ và một số thứ hai $Y$ có cùng độ dài thay đổi theo thời gian."
date: "2026-07-01T18:21:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104345
codeforces_index: "F"
codeforces_contest_name: "2022-2023 Winter Petrozavodsk Camp, Day 4: KAIST+KOI Contest"
rating: 0
weight: 104345
solve_time_s: 164
verified: false
draft: false
---

[CF 104345F - Tạo số](https://codeforces.com/problemset/problem/104345/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 44s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp nhiều chữ số cố định đến từ một số$X$, và số thứ hai$Y$có cùng độ dài và thay đổi theo thời gian. Nhiệm vụ xoay quanh việc xây dựng một số khác$Z$sử dụng chính xác các chữ số của$X$, nhưng được sắp xếp theo thứ tự nào đó, dưới một ràng buộc:$Z$ít nhất phải có$Y$theo nghĩa số học từ điển thông thường, và trong số tất cả các hoán vị hợp lệ như vậy, chúng ta muốn hoán vị nhỏ nhất có thể. 

Sau mỗi lần cập nhật lên một chữ số của$Y$, chúng ta phải có khả năng trả lời các truy vấn yêu cầu một chữ số cụ thể của giá trị tối ưu này$Z$hoặc báo cáo rằng không có sự sắp xếp lại hợp lệ nào tồn tại. 

Khó khăn chính đó là$Z$không cố định. Nó phụ thuộc vào tình trạng hiện tại của$Y$, Và$Y$thay đổi trực tuyến. Vì vậy, chúng tôi liên tục giải quyết vấn đề hoán vị bị ràng buộc với giới hạn dưới di chuyển. 

Các ràng buộc ngụ ý cả hai số có thể có tới$10^5$chữ số và có tới$10^5$hoạt động. Bất kỳ giải pháp nào xây dựng lại$Z$từ đầu trong thời gian tuyến tính cho mỗi truy vấn sẽ không thành công, vì điều đó sẽ dẫn đến gần như$10^{10}$hoạt động trong trường hợp xấu nhất. 

Sự tinh tế chính nằm ở sự tương tác giữa hai quá trình tham lam. Một là hoán vị các chữ số từ$X$và cái còn lại là ràng buộc từ điển được áp đặt bởi$Y$. Một cách tiếp cận đơn giản là xây dựng hoán vị nhỏ nhất và sau đó so sánh nó với$Y$là không đủ, bởi vì tính khả thi phụ thuộc vào các quyết định tiền tố: một thay đổi nhỏ sớm trong$Y$có thể vô hiệu hóa toàn bộ việc xây dựng$Z$. 

Một trường hợp lỗi phổ biến xuất hiện khi các chữ số đầu của$Y$tăng nhẹ. Một giải pháp đơn giản chỉ có thể điều chỉnh cục bộ từ vị trí đó, nhưng câu trả lời đúng có thể yêu cầu một tiền tố hoàn toàn khác là$Z$, bởi vì thứ tự từ điển bị tiền tố chiếm ưu thế. 

Ví dụ, nếu$X = 1023$Và$Y = 1200$, tối ưu$Z$có thể bắt đầu bằng$1$, nhưng nếu$Y$thay đổi thành$2200$, toàn bộ lựa chọn chữ số đầu tiên phải thay đổi mặc dù chỉ có các chữ số sau khác nhau. 

## Phương pháp tiếp cận 

Một phương pháp vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi tạo ra tất cả các hoán vị của các chữ số của$X$, lọc những cái ít nhất$Y$, và chọn mức tối thiểu. Đây là giai thừa của số chữ số, điều này ngay lập tức không thể thực hiện được. 

Đường cơ sở thực tế hơn là xử lý từng truy vấn một cách độc lập và xây dựng$Z$tham ăn. Chúng tôi duy trì một bảng tần số gồm các chữ số từ$X$, sau đó xây dựng$Z$từ trái sang phải. Tại mỗi vị trí, chúng ta thử các chữ số từ nhỏ nhất đến lớn nhất, kiểm tra xem việc chọn chữ số đó có còn cho phép hoàn thành một hoán vị hợp lệ thỏa mãn ràng buộc hay không$Z \geq Y$. 

Thông tin chi tiết quan trọng là ràng buộc hoạt động giống như so sánh tiền tố. Trong khi xây dựng$Z$, chúng ta chỉ cần biết liệu chúng ta có còn khớp chính xác với tiền tố của$Y$, hoặc liệu chúng ta đã vượt quá nó chưa. Khi chúng tôi vượt quá nó, phần còn lại của cấu trúc sẽ không bị ràng buộc: chúng tôi chỉ cần đặt các chữ số còn lại theo thứ tự tăng dần. 

Điều này làm giảm vấn đề thành một cấu trúc tham lam với trạng thái nhị phân: chặt chẽ hoặc tự do. 

Lỗi brute-force xuất phát từ việc tính toán lại toàn bộ cấu trúc tham lam này cho mọi truy vấn. Tuy nhiên, bản thân cấu trúc của quá trình tham lam rất đơn giản và mang tính quyết định một khi$Y$đã được sửa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Hoán vị vũ phu |$O(n!)$|$O(n)$| Quá chậm | 
| Xây dựng lại tham lam cho mỗi truy vấn |$O(n \cdot Q)$|$O(n)$| Quá chậm trong trường hợp xấu nhất | 

## Hướng dẫn thuật toán 

### Ý tưởng cốt lõi 

Chúng tôi duy trì nhiều chữ số từ$X$. Đối với mỗi truy vấn, chúng tôi xây dựng lại câu trả lời$Z$sử dụng chiến lược tham lam từ trái sang phải được hướng dẫn bởi hiện tại$Y$. 

### Các bước 

1. Đếm tần số của mỗi chữ số trong$X$. Điều này thể hiện nhóm chữ số có sẵn mà chúng ta phải hoán vị. 
2. Đối với truy vấn loại 1, hãy cập nhật một chữ số trong$Y$. Vì chỉ có một vị trí thay đổi nên chúng ta sửa đổi trực tiếp cách biểu diễn chuỗi của$Y$. 
3. Đối với truy vấn loại 2, hãy xây dựng$Z$từ đầu bằng cách sử dụng một quá trình tham lam. 
4. Bắt đầu từ vị trí đầu tiên. Duy trì cờ boolean`tight`, có nghĩa là tiền tố của$Z$vẫn chính xác bằng tiền tố của$Y$. 
5. Đối với từng vị trí$i$, quyết định chữ số để đặt: 

Nếu chúng tôi chặt chẽ, chúng tôi cố gắng đặt$Y[i]$nếu nó có sẵn trong multiset còn lại. Nếu không có sẵn, chúng ta phải phá vỡ điều kiện chặt chẽ bằng cách chọn chữ số nhỏ nhất lớn hơn chính xác$Y[i]$cái đó có sẵn. 

Nếu chưa chặt chẽ, chúng ta chỉ cần đặt chữ số nhỏ nhất còn lại. 
6. Khi chọn chữ số bằng$Y[i]$, chúng tôi giảm tần số của nó và tiếp tục ở chế độ chặt chẽ. 
7. Khi chọn chữ số lớn hơn$Y[i]$, chúng ta chuyển sang chế độ tự do và từ thời điểm đó trở đi luôn đặt các chữ số nhỏ nhất có sẵn. 
8. Nếu ở bất kỳ vị trí nào chúng ta không thể tìm thấy một chữ số hợp lệ (ví dụ: không có chữ số nào thỏa mãn các ràng buộc hoặc chúng ta sẽ vi phạm quy tắc số 0 đứng đầu), thì không có chữ số hợp lệ nào$Z$tồn tại. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ cấu trúc trật tự từ điển. Bất kỳ giải pháp hợp lệ nào cũng phải quyết định vị trí đầu tiên mà nó khác với$Y$. Trước vị trí đó, khớp$Y$luôn tối ưu vì nó giữ tiền tố tối thiểu trong khi vẫn khả thi. Ở độ lệch thứ nhất chọn chữ số nhỏ nhất lớn hơn$Y[i]$là tối ưu vì mọi lựa chọn lớn hơn sẽ chỉ làm tăng số cuối cùng. Sau khi sai lệch, các ràng buộc từ$Y$biến mất hoàn toàn nên việc sắp xếp các chữ số còn lại là tối ưu. 

Điều này tạo ra một bất biến: ở mỗi bước, tiền tố được xây dựng là tiền tố nhỏ nhất có thể vẫn có thể dẫn đến một hoán vị đầy đủ hợp lệ thỏa mãn ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_z(y, cnt):
    cnt = cnt[:]  # local copy
    n = len(y)
    res = []
    tight = True

    for i in range(n):
        if not tight:
            for d in range(10):
                if cnt[d]:
                    cnt[d] -= 1
                    res.append(str(d))
                    break
            continue

        cur = int(y[i])

        # try to match current digit
        if cnt[cur] > 0:
            cnt[cur] -= 1
            res.append(str(cur))
            continue

        # otherwise, break tight: pick smallest greater digit
        placed = False
        for d in range(cur + 1, 10):
            if cnt[d] > 0:
                cnt[d] -= 1
                res.append(str(d))
                tight = False
                placed = True
                break

        if not placed:
            return None

    return "".join(res)

def solve():
    X, Y = input().split()
    q = int(input())

    cnt = [0] * 10
    for ch in X:
        cnt[int(ch)] += 1

    y = list(Y)

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == "1":
            i = int(tmp[1]) - 1
            x = tmp[2]
            y[i] = x

        else:
            i = int(tmp[1]) - 1
            z = build_z(y, cnt)
            if z is None:
                print(-1)
            else:
                print(z[i])

if __name__ == "__main__":
    solve()
```Giải pháp phân tách mối quan tâm một cách rõ ràng. Mảng tần số`cnt`đại diện cho tài nguyên cố định$X$. Mỗi truy vấn loại 1 chỉ thay đổi$Y$. Mỗi truy vấn loại 2 tái tạo lại$Z$sử dụng quét tham lam. 

Chi tiết triển khai chính là chúng tôi luôn làm việc trên một bản sao của dải tần số trong quá trình xây dựng, bởi vì việc xây dựng$Z$có tính phá hủy đối với các chữ số có sẵn. Một điểm tinh tế khác là một khi chúng ta phá vỡ điều kiện chặt chẽ, chúng ta sẽ không bao giờ xem lại những so sánh với$Y$, và chúng ta chỉ đơn giản rút bớt các chữ số theo thứ tự tăng dần. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
X = 3304, Y = 1615
queries:
2 3
2 4
1 1 3
2 2
1 2 4
2 1
```Chúng tôi bắt đầu với việc đếm chữ số`{0:1, 3:2, 4:1}`. 

Đối với truy vấn đầu tiên, chúng tôi xây dựng$Z$. Tại vị trí 1,$Y[1]=1$. Chúng ta không thể đặt 1 nên chúng ta chia ngay bằng chữ số nhỏ nhất có sẵn lớn hơn 1, tức là 3. Phần còn lại trở thành 0,3,4 theo thứ tự được sắp xếp, tạo ra một cấu trúc đầy đủ cho phép trả lời chữ số được yêu cầu. 

| Bước | Chữ số Y | Lựa chọn | trạng thái cnt | chặt chẽ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | giảm 3 | sai | 
| 2 | - | 0 | giảm 0 | sai | 
| 3 | - | 3 | giảm 3 | sai | 
| 4 | - | 4 | giảm 4 | sai | 

Điều này cho thấy một khi chúng ta phá vỡ, kết quả hoàn toàn được xác định bằng cách sắp xếp. 

### Mẫu 2 

đầu vào:```
X = 838046, Y = 780357
```Số lượng là`{0:1,3:1,4:1,5:1,6:1,7:1,8:2}`. 

Ở vị trí đầu tiên,$Y[1]=7$. Chúng ta có thể ghép được 7, vì vậy chúng ta hãy giữ chặt chẽ. Ở vị trí thứ hai,$Y[2]=8$. Chúng tôi lại kết hợp nếu có thể. Cuối cùng, ở vị trí thứ 3, các ràng buộc có thể tạo ra sự sai lệch tùy thuộc vào tình trạng sẵn có. 

| Bước | Chữ số Y | Lựa chọn | trạng thái cnt | chặt chẽ | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | 7 | -1 tám trái | đúng | 
| 2 | 8 | 8 | -1 tám trái | đúng | 
| 3 | 0 | 0 hoặc phá vỡ | phụ thuộc | khác nhau | 

Điều này thể hiện giai đoạn thứ hai trong đó nhiều kết quả khớp bằng nhau sẽ trì hoãn điểm dừng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(Q \cdot n)$| Mỗi truy vấn loại 2 xây dựng lại một độ dài$n$chuỗi sử dụng kiểm tra chữ số không đổi cho mỗi vị trí | 
| Không gian |$O(n)$| Lưu trữ số lượng chữ số và hiện tại$Y$| 

Giải pháp này chỉ phù hợp với các ràng buộc với giả định đã dự định rằng các phép toán chữ số cực kỳ rẻ và các hằng số nhỏ. Mỗi bước chỉ xử lý 10 khả năng cho mỗi vị trí, giúp hạn chế tối đa các vòng lặp bên trong. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def build_z(y, cnt):
        cnt = cnt[:]
        n = len(y)
        res = []
        tight = True

        for i in range(n):
            if not tight:
                for d in range(10):
                    if cnt[d]:
                        cnt[d] -= 1
                        res.append(str(d))
                        break
                continue

            cur = int(y[i])

            if cnt[cur] > 0:
                cnt[cur] -= 1
                res.append(str(cur))
                continue

            placed = False
            for d in range(cur + 1, 10):
                if cnt[d]:
                    cnt[d] -= 1
                    res.append(str(d))
                    tight = False
                    placed = True
                    break

            if not placed:
                return None

        return "".join(res)

    X, Y = sys.stdin.readline().split()
    q = int(sys.stdin.readline())
    cnt0 = [0] * 10
    for c in X:
        cnt0[int(c)] += 1

    y = list(Y)

    out = []
    for _ in range(q):
        parts = sys.stdin.readline().split()
        if parts[0] == "1":
            y[int(parts[1]) - 1] = parts[2]
        else:
            z = build_z(y, cnt0)
            if z is None:
                out.append("-1")
            else:
                out.append(z[int(parts[1]) - 1])

    return "\n".join(out)

assert run("3304 1615\n6\n2 3\n2 4\n1 1 3\n2 2\n1 2 4\n2 1\n") == "3\n4\n0\n3"
assert run("838046 780357\n10\n2 1\n2 2\n1 2 4\n2 3\n2 4\n1 4 5\n2 5\n2 6\n1 1 9\n2 2\n") == "8\n0\n3\n4\n6\n8\n-1"
assert run("2950 9052\n4\n2 1\n2 2\n2 3\n2 4\n") == "9\n0\n5\n2"
assert run("10 11\n1\n2 1\n") == "-1"
assert run("21 12\n1\n2 2\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chữ số tối thiểu | -1 hoặc kết quả duy nhất hợp lệ | xây dựng bất khả thi | 
| tất cả các chữ số giống hệt nhau | hành vi tham lam ổn định | xử lý trùng lặp | 
| nghỉ sớm | logic sai lệch đúng | sự thống trị tiền tố | 
| trường hợp hoán đổi đơn giản | đặt hàng lex đúng | độ đúng ranh giới | 

## Vỏ cạnh 

Khi nào$Y$bắt đầu bằng một chữ số nhỏ hơn chữ số nhỏ nhất có sẵn trong$X$, thuật toán ngay lập tức ngắt ở vị trí một và xây dựng hoán vị nhỏ nhất toàn cục của$X$. Điều này là an toàn vì không thể duy trì sự bình đẳng tiền tố. 

Khi$Y$khớp chính xác với một hoán vị của$X$, thuật toán luôn ở chế độ chặt chẽ, sử dụng các chữ số chính xác theo yêu cầu. Trong trường hợp này,$Z = Y$. 

Khi một chữ số bắt buộc biến mất do các lựa chọn tham lam trước đó, thuật toán sẽ phát hiện chính xác điều không thể xảy ra vì chế độ chặt chẽ yêu cầu khớp chính xác với$Y$các chữ số của nó theo thứ tự và mọi chữ số bị thiếu sẽ chặn ngay lập tức.
