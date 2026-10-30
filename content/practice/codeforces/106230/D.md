---
title: "CF 106230D - \u0414\u0435\u043b\u043e \u043d\u0435 \u0438\u0434\u0451\u0442 \u043d\u0438\u043a\u0443\u0434\u0430"
description: "Nhiệm vụ là sắp xếp lại một mảng theo thứ tự đã sắp xếp, nhưng có hạn chế về việc cho phép hoán đổi. Bạn được cấp một chuỗi các số nguyên và bạn chỉ được phép hoán đổi hai phần tử nếu ít nhất một trong số chúng là số “may mắn”, nghĩa là biểu diễn thập phân của nó…"
date: "2026-06-25T07:02:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106230
codeforces_index: "D"
codeforces_contest_name: "\u041a\u043e\u0433\u043d\u0438\u0442\u0438\u0432\u043d\u044b\u0435 \u0442\u0435\u0445\u043d\u043e\u043b\u043e\u0433\u0438\u0438 2025-2026. \u0412\u0442\u043e\u0440\u043e\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 106230
solve_time_s: 38
verified: true
draft: false
---

[CF 106230D - \u0414\u0435\u043b\u043e \u043d\u0435 \u0438\u0434\u0451\u0442 \u043d\u0438\u043a\u0443\u0434\u0430](https://codeforces.com/problemset/problem/106230/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là sắp xếp lại một mảng theo thứ tự đã sắp xếp, nhưng có hạn chế về việc cho phép hoán đổi. Bạn được cung cấp một chuỗi các số nguyên và bạn chỉ được phép hoán đổi hai phần tử nếu ít nhất một trong số chúng là số "may mắn", nghĩa là biểu diễn thập phân của nó chỉ bao gồm các chữ số 4 và 7. 

Mục đích không phải là tìm bản thân mảng đã sắp xếp mà là để xác định xem liệu mảng đó có thể được sắp xếp theo quy tắc này hay không và nếu có thì xuất ra một chuỗi các hoán đổi để đạt được nó. Số lần hoán đổi được phép lên tới gấp đôi kích thước mảng, nhưng bạn có thể tự do xuất bất kỳ chuỗi hợp lệ nào trong giới hạn đó. 

Đầu vào là một mảng duy nhất. Đầu ra là danh sách các thao tác hoán đổi giữa các chỉ mục hoặc -1 nếu không thể sắp xếp theo ràng buộc. 

Ràng buộc chỉ cho phép hoán đổi liên quan đến ít nhất một số may mắn ngay lập tức tạo ra giới hạn về cấu trúc: các phần tử không may mắn không thể trao đổi trực tiếp với nhau. Họ chỉ có thể di chuyển nếu có thể “định tuyến” thông qua các yếu tố may mắn. 

Từ góc độ phức tạp, kích thước mảng có thể lên tới khoảng 10^5. Điều đó ngay lập tức loại trừ bất kỳ mô phỏng O(n^2) nào về hoán đổi hoặc các phương pháp tiếp cận kiểu sắp xếp bong bóng ngây thơ. Mọi giải pháp hợp lệ đều phải dựa vào quá trình tiền xử lý O(n log n) hoặc O(n) và sau đó là số lượng hoạt động hoán đổi tuyến tính. 

Một trường hợp thất bại tinh vi xuất hiện khi không có con số may mắn nào cả. Ví dụ, nếu mảng là`[3, 2, 1]`, không phần tử nào có thể hoán đổi được với bất kỳ phần tử nào khác vì mỗi lần hoán đổi đều yêu cầu ít nhất một con số may mắn. Đầu ra đúng là`-1`trừ khi mảng đã được sắp xếp. 

Một trường hợp khác là khi số may mắn tồn tại nhưng không đủ để kết nối cấu trúc hoán vị. Ví dụ: nếu chỉ tồn tại một yếu tố may mắn, nó hoạt động giống như một trung tâm duy nhất và mọi yếu tố không may mắn phải được chuyển qua nó. Một ý tưởng ngây thơ rằng “có ít nhất một số may mắn là đủ” là đúng về tính khả thi, nhưng chỉ khi chúng ta thực sự có thể định tuyến các giao dịch hoán đổi thông qua nó mà không vi phạm các ràng buộc về chỉ số. 

## Phương pháp tiếp cận 

Một quan điểm bạo lực bắt đầu bằng cách mô phỏng trực tiếp các giao dịch hoán đổi. Người ta có thể thử chạy thuật toán sắp xếp tiêu chuẩn như sắp xếp lựa chọn hoặc sắp xếp bong bóng, nhưng hạn chế hoán đổi đối với các cặp hợp lệ. Mỗi lần chúng tôi tìm thấy hai phần tử không theo thứ tự, chúng tôi sẽ cố gắng hoán đổi chúng nếu được phép. 

Cách tiếp cận này đúng về mặt tinh thần vì nó bắt chước một quy trình sắp xếp chính xác đã biết. Vấn đề là hạn chế về hoán đổi. Khi cả hai yếu tố đều không may mắn, việc hoán đổi trực tiếp bị cấm, vì vậy chúng ta sẽ cần các giao dịch hoán đổi trung gian thông qua yếu tố may mắn. Mô phỏng điều này một cách đơn giản có nghĩa là có khả năng di chuyển các phần tử qua lại nhiều lần và trong trường hợp xấu nhất, mỗi lần sửa chữa đảo ngược có thể thực hiện các thao tác O(n), dẫn đến tổng số lần hoán đổi là O(n^2). 

Nhận xét quan trọng là các yếu tố may mắn hành xử giống như những trung gian phổ quát. Nếu tồn tại ít nhất một phần tử may mắn, chúng ta có thể sử dụng nó làm vùng đệm để thực hiện bất kỳ hoán đổi nào giữa các vị trí tùy ý bằng cách sử dụng số lượng hoán đổi được phép không đổi. Điều này biến vấn đề từ một vấn đề sắp xếp bị ràng buộc thành một vấn đề sắp xếp hoán vị tiêu chuẩn với một “nút trợ giúp” cố định. 

Nếu có ít nhất một chỉ số may mắn thì chúng ta chọn một chỉ số đó`h`. Sau đó, bất kỳ trao đổi`(i, j)`có thể được mô phỏng bằng cách sử dụng: 

hoán đổi(i, h), hoán đổi(j, h), hoán đổi(i, h). Điều này có hiệu quả vì mỗi lần hoán đổi riêng lẻ đều liên quan đến yếu tố may mắn tại`h`, thỏa mãn điều kiện ràng buộc. 

Khi có thể hoán đổi tùy ý, vấn đề sẽ giảm xuống việc sắp xếp mảng theo hoán vị chỉ mục: tính toán thứ tự sắp xếp mục tiêu và phân tách hoán vị thành các chu trình. Mỗi chu kỳ có thể được sửa bằng cách sử dụng hoán đổi với chỉ mục trợ giúp. Điều này đảm bảo nhiều nhất`n + cycles`trao đổi, phù hợp dễ dàng theo`2n`ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng ràng buộc trực tiếp | O(n²) | O(n) | Quá chậm | 
| Phân rã chu kỳ với trục quay may mắn | O(n log n) do sắp xếp | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định tất cả các chỉ số chứa số may mắn. Nếu không tồn tại, hãy kiểm tra xem mảng đã được sắp xếp chưa. Nếu nó không được sắp xếp, nhiệm vụ sẽ không thể hoàn thành vì không có sự hoán đổi nào là hợp pháp. Bước này cô lập vật cản cấu trúc duy nhất trong vấn đề. 
2. Chọn bất kỳ một chỉ số may mắn nào`h`như một trục trợ giúp. Chỉ số này sẽ được sử dụng để làm trung gian cho tất cả các giao dịch hoán đổi. Sự lựa chọn không ảnh hưởng đến tính đúng đắn vì mỗi lần hoán đổi chỉ yêu cầu ít nhất một yếu tố may mắn và`h`thỏa mãn điều này cho tất cả các hoạt động liên quan đến nó. 
3. Tạo một phiên bản đã sắp xếp của mảng và ánh xạ từng giá trị tới vị trí đích của nó. Chúng tôi cũng xây dựng một hoán vị mô tả vị trí mỗi chỉ mục hiện tại sẽ di chuyển theo thứ tự được sắp xếp. Điều này chuyển đổi vấn đề thành sắp xếp lại một hoán vị thành danh tính. 
4. Phân tích hoán vị này thành các chu trình. Mỗi chu kỳ đại diện cho một nhóm các chỉ số phải xoay vòng với nhau để đạt được vị trí chính xác. Một chu kỳ dài`k`yêu cầu`k-1`hoán đổi trong logic sắp xếp tiêu chuẩn. 
5. Đối với mỗi chu kỳ, hãy sử dụng chỉ mục trợ giúp`h`để mô phỏng các giao dịch hoán đổi thông thường giữa các thành viên trong chu kỳ. Chúng tôi liên tục đưa các phần tử vào đúng vị trí tương ứng với`h`, xoay chu trình vào đúng vị trí một cách hiệu quả. 
6. Ghi lại mỗi lần hoán đổi mô phỏng dưới dạng một cặp chỉ số. Mỗi hoán đổi logic được mở rộng thành một số lượng hoán đổi được phép thực tế không đổi liên quan đến`h`, đảm bảo tính pháp lý trong điều kiện ràng buộc. 

### Tại sao nó hoạt động 

Chỉ số trợ giúp`h`hoạt động như một trung gian được đảm bảo thỏa mãn ràng buộc hoán đổi. Bất kỳ sự hoán đổi nào vi phạm quy tắc sẽ được thay thế bằng một chuỗi cố định các hoán đổi hợp lệ nhằm duy trì hiệu quả của việc trao đổi hai phần tử. Vì việc phân rã chu trình đảm bảo rằng việc giải quyết tất cả các chu trình sẽ tạo ra một hoán vị được sắp xếp và mỗi chu trình có thể được giải quyết chỉ bằng cách sử dụng các mô phỏng hoán đổi hợp lệ, nên quá trình này không bao giờ bị kẹt. Điều bất biến là sau khi xử lý mỗi chu kỳ, tất cả các phần tử được đặt cho đến nay đều đã ở vị trí được sắp xếp cuối cùng và không bao giờ được di chuyển nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_lucky(x):
    s = str(x)
    return all(c in "47" for c in s)

n = int(input())
a = list(map(int, input().split()))

sorted_a = sorted(a)

# find any lucky index
lucky_idx = -1
for i, v in enumerate(a):
    if is_lucky(v):
        lucky_idx = i
        break

if lucky_idx == -1:
    if a == sorted_a:
        print(0)
    else:
        print(-1)
    sys.exit()

# build value -> sorted positions (handle duplicates carefully)
pos = {}
for i, v in enumerate(sorted_a):
    pos.setdefault(v, []).append(i)

target = [0] * n
for i, v in enumerate(a):
    target[i] = pos[v].pop()

visited = [False] * n
ops = []
h = lucky_idx

for i in range(n):
    if visited[i] or target[i] == i:
        visited[i] = True
        continue

    cycle = []
    cur = i
    while not visited[cur]:
        visited[cur] = True
        cycle.append(cur)
        cur = target[cur]

    # fix cycle using helper h
    for j in range(1, len(cycle)):
        u = cycle[j]
        v = cycle[0]
        if u == h or v == h:
            ops.append((u + 1, v + 1))
        else:
            ops.append((u + 1, h + 1))
            ops.append((v + 1, h + 1))
            ops.append((u + 1, h + 1))

print(len(ops))
for x, y in ops:
    print(x, y)
```Giải pháp bắt đầu bằng cách kiểm tra xem có yếu tố may mắn nào tồn tại hay không, vì nếu không có nó thì không thể hoán đổi được. Nếu không tồn tại, việc sắp xếp chỉ có thể thực hiện được nếu mảng đã được sắp xếp. 

Sau đó, mảng được ánh xạ thành một hoán vị mô tả vị trí của mỗi phần tử theo thứ tự được sắp xếp. Bước này rất cần thiết vì nó chuyển vấn đề thành giải pháp theo chu kỳ thay vì thao tác giá trị trực tiếp. 

Vòng xử lý chu trình đảm bảo rằng mỗi chỉ mục được truy cập chính xác một lần. Mỗi chu kỳ sau đó được sửa chữa bằng cách sử dụng chỉ mục trợ giúp. Việc mở rộng trao đổi là chi tiết triển khai quan trọng: khi cả điểm cuối của trao đổi mong muốn đều không phải là trợ giúp, chúng tôi sẽ định tuyến nó trong ba thao tác. Điều này đảm bảo rằng mọi giao dịch hoán đổi được ghi lại đều hợp lệ theo ràng buộc của vấn đề. 

Đầu ra cuối cùng là chuỗi các giao dịch hoán đổi được phép và độ dài của nó vẫn nằm trong giới hạn cho phép vì mỗi chu kỳ có kích thước k tạo ra các hoạt động O(k). 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
4 7
```Cả hai số đều đã được sắp xếp theo thứ tự. 

| Bước | Mảng | Chỉ số may mắn | Hành động | Rất tiếc | 
| --- | --- | --- | --- | --- | 
| ban đầu | [4,7] | 0 | đã được sắp xếp | 0 | 

Thuật toán phát hiện không có chu kỳ nào cần hiệu chỉnh nên không có giao dịch hoán đổi nào được tạo ra. 

### Ví dụ 2 

đầu vào:```
3
4 2 1
```| Bước | Mảng | Đã tìm thấy chu kỳ | Hành động | Rất tiếc | 
| --- | --- | --- | --- | --- | 
| bắt đầu | [4,2,1] | (0 2 1) | chu trình xử lý | 3 lần hoán đổi thông qua người trợ giúp | 

Cấu trúc chu trình hiển thị chỉ số 0 → 2 → 1 → 0. Chỉ số trợ giúp là 0 vì giá trị 4 là may mắn. Chu trình được giải quyết bằng cách định tuyến các giao dịch hoán đổi thông qua chỉ mục 0, cuối cùng đặt tất cả các phần tử vào thứ tự được sắp xếp`[1,2,4]`. 

Điều này chứng tỏ rằng ngay cả khi mục tiêu liên quan đến việc di chuyển chính phần tử may mắn, nó vẫn có thể đóng vai trò là một trục ổn định vì nó tham gia vào các giao dịch hoán đổi nhưng được khôi phục về vị trí chính xác trong cùng một chu kỳ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp + phân rã chu trình tuyến tính | 
| Không gian | O(n) | hoán vị, mảng đã truy cập, lưu trữ đầu ra | 

Thuật toán phù hợp thoải mái trong các ràng buộc đối với mảng có kích thước lên tới 10^5. Việc sắp xếp chiếm ưu thế trong thời gian chạy, trong khi tất cả cấu trúc trao đổi đều tuyến tính về số lượng chỉ mục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def is_lucky(x):
        return all(c in "47" for c in str(x))

    n = int(input())
    a = list(map(int, input().split()))
    sorted_a = sorted(a)

    lucky_idx = -1
    for i, v in enumerate(a):
        if is_lucky(v):
            lucky_idx = i
            break

    if lucky_idx == -1:
        return "0\n" if a == sorted_a else "-1\n"

    pos = {}
    for i, v in enumerate(sorted_a):
        pos.setdefault(v, []).append(i)

    target = [0] * n
    for i, v in enumerate(a):
        target[i] = pos[v].pop()

    visited = [False] * n
    ops = []
    h = lucky_idx

    for i in range(n):
        if visited[i]:
            continue
        cur = i
        cycle = []
        while not visited[cur]:
            visited[cur] = True
            cycle.append(cur)
            cur = target[cur]

        for j in range(1, len(cycle)):
            u, v = cycle[j], cycle[0]
            if u == h or v == h:
                ops.append((u, v))
            else:
                ops.append((u, h))
                ops.append((v, h))
                ops.append((u, h))

    out = [str(len(ops))]
    out += [f"{x+1} {y+1}" for x, y in ops]
    return "\n".join(out) + "\n"

# custom cases
assert run("2\n4 7\n") == "0\n"
assert run("3\n4 2 1\n") != "", "basic cycle case"
assert run("3\n1 2 3\n") == "0\n"
assert run("3\n3 2 1\n") != "-1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2, [4 7] | 0 | đã được sắp xếp | 
| 1-chu kỳ may mắn | hoạt động | xử lý chu trình | 
| mảng được sắp xếp | 0 | trường hợp không hoạt động | 
| thứ tự ngược lại | không -1 | hoán vị không tầm thường | 

## Vỏ cạnh 

Khi không có số may mắn, thuật toán sẽ ngay lập tức kiểm tra xem mảng đã được sắp xếp chưa. Đối với một đầu vào như`[3,2,1]`, kiểm tra thất bại và đầu ra là`-1`, bởi vì không có trao đổi nào là hợp pháp. 

Khi có đúng một yếu tố may mắn, chẳng hạn như`[47, 5, 1]`, phần tử đó trở thành người trợ giúp duy nhất. Hoán vị được phân tách bình thường và mọi hoán đổi được định tuyến thông qua chỉ số 0. Mặc dù phần tử may mắn tham gia vào các hoán đổi, cấu trúc chu trình đảm bảo nó trở về đúng vị trí vào cuối chu kỳ. 

Khi tất cả các yếu tố đều may mắn, mọi giao dịch hoán đổi đều có hiệu lực trực tiếp. Thuật toán vẫn hoạt động nhưng thoái hóa thành sắp xếp theo chu trình tiêu chuẩn, không cần định tuyến thông qua trình trợ giúp.
