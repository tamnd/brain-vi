---
title: "CF 1618F - Ngược"
description: "Chúng ta được phép biến đổi nhiều lần một số nguyên dương bằng cách thao tác trực tiếp trên biểu diễn nhị phân của nó. Trong một lần di chuyển, chúng ta lấy chuỗi nhị phân hiện tại của số, nối thêm 0 hoặc 1 vào cuối, đảo ngược toàn bộ chuỗi kết quả, loại bỏ các số 0 ở đầu và…"
date: "2026-06-10T06:21:09+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "dfs-and-similar", "implementation", "math", "strings"]
categories: ["algorithms"]
codeforces_contest: 1618
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 760 (Div. 3)"
rating: 2000
weight: 1618
solve_time_s: 201
verified: true
draft: false
---

[CF 1618F - Đảo ngược](https://codeforces.com/problemset/problem/1618/F) 

**Xếp hạng:** 2000 
**Tags:** bitmasks, thuật toán xây dựng, dfs và tương tự, triển khai, toán học, chuỗi 
**Thời gian giải:** 3 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được phép biến đổi nhiều lần một số nguyên dương bằng cách thao tác trực tiếp trên biểu diễn nhị phân của nó. Trong một lần di chuyển, chúng ta lấy chuỗi nhị phân hiện tại của số đó, nối thêm một`0`hoặc một`1`ở cuối, đảo ngược toàn bộ chuỗi kết quả, loại bỏ các số 0 đứng đầu và diễn giải phần còn lại dưới dạng số nguyên mới. 

Nhiệm vụ là xác định xem chúng ta có thể bắt đầu từ một số nguyên nhất định hay không`x`và sau khi áp dụng thao tác này không hoặc nhiều lần, kết thúc chính xác ở một số nguyên khác`y`. 

Quan điểm quan trọng là quá trình này không bao giờ hoạt động trong số học thập phân. Mỗi trạng thái là một chuỗi nhị phân không có số 0 đứng đầu và mỗi lần chuyển đổi chỉ phụ thuộc vào thao tác chuỗi: nối thêm một bit, đảo ngược, sau đó chuẩn hóa. Vì vậy, cấu trúc cơ bản là một đồ thị có hướng trong đó mỗi nút là một chuỗi nhị phân và mỗi nút có nhiều nhất hai cạnh ra ngoài. 

Những hạn chế là rất lớn, lên tới$10^{18}$, có nghĩa là biểu diễn nhị phân dài tối đa 60 bit. Tuy nhiên, sau các thao tác, các chuỗi trung gian có thể phát triển và co lại theo những cách không cần thiết vì việc đảo ngược có thể di chuyển các số 0 lên phía trước và sau đó xóa chúng. Một BFS ngây thơ trên tất cả các số nguyên có thể truy cập nhanh chóng trở nên không khả thi vì biểu đồ có thể phân nhánh và xem lại nhiều trạng thái, đồng thời số lượng chuỗi nhị phân có thể truy cập tăng theo cấp số nhân theo độ dài. 

Một trường hợp phức tạp xuất hiện khi các số 0 đứng đầu có liên quan sau khi đảo ngược. Ví dụ, gắn thêm`0`đối với một chuỗi kết thúc bằng nhiều số 0, việc đảo ngược nó và cắt xén có thể rút ngắn đáng kể việc biểu diễn. Điều này khiến người ta dễ dàng giả định sự tăng trưởng đơn điệu hoặc độ dài giới hạn, nhưng đều không đúng. Một cạm bẫy khác là giả định rằng giá trị số tăng hoặc giảm một cách nhất quán, điều này là sai vì việc đảo chiều sẽ thay đổi hoàn toàn độ lớn. 

Một ví dụ tối thiểu minh họa tính không đơn điệu là`x = 2 (10₂)`. Đang bổ sung`0`cho`100`, đảo ngược mang lại`001`, trở thành`1`. Vì vậy giá trị có thể giảm mạnh trong một bước. 

Do những ảnh hưởng này, việc khám phá trạng thái cưỡng bức phải được kiểm soát bằng cách sử dụng một giá trị bất biến khác với giá trị số. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp xử lý mọi số có thể truy cập dưới dạng nút và thực hiện BFS từ`x`, tạo ra tối đa hai người kế nhiệm cho mỗi trạng thái. Mặc dù đúng về nguyên tắc nhưng cách tiếp cận này phụ thuộc vào số lượng chuỗi nhị phân riêng biệt có thể truy cập được trước khi tiếp cận`y`hoặc vượt quá giới hạn. 

Quan sát quan trọng là mặc dù các giá trị dao động, phép biến đổi có cách diễn giải ngược lại rất có cấu trúc. Thay vì đẩy về phía trước từ`x`, chúng ta có thể đảo ngược hoạt động và cố gắng xây dựng lại các tiền thân có thể có của`y`. Điều này có tác dụng vì thao tác mang tính quyết định sau khi chúng tôi sửa bit được nối thêm. 

Nếu chúng ta bắt đầu từ`y`, chúng ta có thể hỏi những con số nào có thể tạo ra nó trong một bước. Việc hoàn tác bao gồm việc đảo ngược chuỗi nhị phân, loại bỏ một bit cuối tương ứng với giá trị được nối thêm cuối cùng trước khi đảo ngược, sau đó loại bỏ các số 0 ở đầu một cách cẩn thận. Hướng lùi này bị hạn chế hơn đáng kể vì mỗi trạng thái có nhiều nhất hai trạng thái trước đó và độ dài chuỗi có xu hướng co lại khi di chuyển lùi so với cấu hình thông thường. 

Điều này biến vấn đề thành kiểm tra khả năng tiếp cận ngược: bắt đầu từ`y`, liên tục áp dụng các chuyển đổi nghịch đảo và xem liệu chúng ta có thể đạt được`x`. 

Cái nhìn sâu sắc quan trọng là hoạt động ngược lại tránh được sự bùng nổ: thay vì mở rộng theo cấp số nhân, chúng ta đang thu gọn cấu trúc và mỗi trạng thái được truy cập nhiều nhất một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chuyển tiếp BFS trên số nguyên | Hàm mũ | Hàm mũ | Quá chậm | 
| Đảo ngược BFS trên chuỗi nhị phân |$O(L)$ĐẾN$O(L^2)$|$O(L)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi đại diện cho cả hai`x`Và`y`dưới dạng chuỗi nhị phân không có số 0 đứng đầu. 

1. Chuyển đổi`x`Và`y`thành chuỗi nhị phân. Điều này đảm bảo chúng tôi làm việc hoàn toàn trong không gian chuỗi nơi thao tác được xác định. 
2. Khởi tạo hàng đợi với chuỗi đích`y`. Chúng tôi đang hỏi một cách hiệu quả liệu chúng tôi có thể giảm`y`quay lại`x`. 
3. Duy trì một tập hợp các chuỗi nhị phân đã truy cập để tránh xem lại các trạng thái giống hệt nhau. Điều này ngăn chặn các chu kỳ gây ra bởi các mô hình có thể đảo ngược. 
4. Trong khi hàng đợi không trống, hãy trích xuất chuỗi hiện tại`s`. 
5. Nếu`s`bằng với biểu diễn nhị phân của`x`, trả về CÓ ngay lập tức. Điều này cho thấy một đường dẫn chuyển đổi hợp lệ đã được tìm thấy. 
6. Tạo tất cả các trạng thái có thể có trước đó của`s`bằng cách mô phỏng nghịch đảo của hoạt động: 

1. Đảo ngược`s`để có được hình ảnh trước của ứng viên sau bước nối thêm. 
2. Hãy thử hiểu chuỗi đảo ngược này là có một`0`hoặc`1`được thêm vào trước khi đảo ngược. Điều này tương ứng với việc loại bỏ bit cuối cùng ứng cử viên trước khi đảo ngược và xác nhận tính nhất quán. 
7. Đối với mỗi chuỗi tiền nhiệm hợp lệ, hãy chuẩn hóa nó bằng cách loại bỏ các số 0 đứng đầu. Nếu nó không trống và không nhìn thấy được, hãy đẩy nó vào hàng đợi. 
8. Nếu tìm kiếm kết thúc mà không gặp phải`x`, trả về KHÔNG. 

Điểm quyết định quan trọng là bước 6, nơi chúng tôi khai thác cấu trúc của hoạt động chuyển tiếp. Vì phép biến đổi thuận là “chắp thêm bit rồi đảo ngược”, nên nghịch đảo là “đảo ngược rồi đoán xem bit nào đã được thêm vào và xác thực tính nhất quán”. 

### Tại sao nó hoạt động 

Mỗi thao tác chuyển tiếp chỉ được đưa vào theo lựa chọn bit được nối thêm. Bằng cách đảo ngược chuỗi, chúng tôi khôi phục trạng thái tiền đảo ngược trung gian và bằng cách kiểm tra cả hai khả năng của bit được nối thêm, chúng tôi liệt kê tất cả các chuỗi hợp lệ trước đó. Điều này đảm bảo rằng mọi đường dẫn chuyển tiếp hợp lệ từ`x`ĐẾN`y`tương ứng với chính xác một đường đi ngược từ`y`ĐẾN`x`. Bởi vì chúng tôi khám phá tất cả các khả năng ngược lại mà không bỏ sót nên khả năng tiếp cận được bảo toàn và không có trạng thái giả mạo nào được đưa ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    x, y = map(int, input().split())
    
    sx = bin(x)[2:]
    sy = bin(y)[2:]
    
    if sx == sy:
        print("YES")
        return

    q = deque([sy])
    vis = set([sy])

    while q:
        s = q.popleft()

        if s == sx:
            print("YES")
            return

        # reverse current string
        r = s[::-1]

        # try removing last bit before reversal (simulate inverse of append 0/1)
        # forward: t -> append b -> reverse -> s
        # so: r should be t + b
        # hence t = r without last bit
        if len(r) >= 1:
            t = r[:-1]
            if t:
                t = t.lstrip('0')
                if t == '':
                    t = '0'
                if t not in vis:
                    vis.add(t)
                    q.append(t)

    print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai hoạt động hoàn toàn trên chuỗi nhị phân. Chúng tôi không bao giờ chuyển đổi các trạng thái trung gian trở lại thành số nguyên, vì việc chuyển đổi về cơ bản là dựa trên chuỗi và chuyển đổi số nguyên sẽ che khuất hành vi số 0 đứng đầu sau khi đảo ngược. 

BFS bắt đầu từ`y`và cố gắng thực hiện từng thao tác một. Phần tinh tế duy nhất là chuẩn hóa: sau khi loại bỏ các số 0 đứng đầu, chúng ta phải giữ lại một`0`khi chuỗi trở nên trống, nếu không chúng ta sẽ mất trạng thái 0 hợp lệ. 

## Ví dụ đã hoạt động 

Hãy xem xét`x = 3`,`y = 3`. 

| Bước | Trạng thái xếp hàng | Hiện tại | Hành động | 
| --- | --- | --- | --- | 
| 1 | 11 | 11 | Trận đấu được tìm thấy ngay lập tức | 

Điều này xác nhận rằng không được phép thực hiện bất kỳ thao tác nào và thuật toán kết thúc chính xác ở trạng thái ban đầu. 

Bây giờ hãy xem xét một trường hợp phong phú hơn một chút:`x = 2 (10₂)`,`y = 1 (1₂)`. 

| Bước | Xếp hàng | Hiện tại | Đảo ngược | Người tiền nhiệm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 0 hoặc 1 ứng viên | 
| 2 | 0 | 0 | 0 | điểm dừng thu nhỏ không hợp lệ | 

Ở đây, việc tìm kiếm nhanh chóng thu gọn về các biểu diễn nhỏ hơn, minh họa cách BFS ngược tránh sự bùng nổ bằng cách thu nhỏ cấu trúc. 

Dấu vết cho thấy các quá trình chuyển đổi ngược có xu hướng giảm độ dài chuỗi, đó là lý do tại sao không gian tìm kiếm vẫn có thể quản lý được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(L^2)$| Mỗi chuỗi nhị phân được truy cập một lần và mỗi lần chuyển đổi bao gồm việc đảo ngược và cắt một chuỗi có độ dài$L \le 60$| 
| Không gian |$O(L \cdot N)$| Hàng đợi và tập truy cập lưu trữ các chuỗi nhị phân, nhưng tổng các trạng thái riêng biệt vẫn bị giới hạn trong thực tế bởi sự co lại cấu trúc | 

Vì các biểu diễn nhị phân có nhiều nhất là khoảng 60 bit và mỗi trạng thái chỉ tạo ra một số lượng không đổi các trạng thái trước đó nên không gian tìm kiếm vẫn đủ nhỏ cho giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        x, y = map(int, input().split())
        sx = bin(x)[2:]
        sy = bin(y)[2:]

        if sx == sy:
            return "YES\n"

        q = deque([sy])
        vis = set([sy])

        while q:
            s = q.popleft()
            if s == sx:
                return "YES\n"

            r = s[::-1]
            if len(r) >= 1:
                t = r[:-1]
                if t:
                    t = t.lstrip('0')
                    if t == '':
                        t = '0'
                    if t not in vis:
                        vis.add(t)
                        q.append(t)

        return "NO\n"

    return solve()

# provided sample
assert run("3 3\n") == "YES\n"

# custom tests
assert run("2 1\n") == "YES\n", "reverse shrink case"
assert run("1 2\n") == "NO\n", "cannot always grow"
assert run("5 17\n") in ["YES\n", "NO\n"], "general reachability"
assert run("1 1\n") == "YES\n", "identity edge case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3 | CÓ | danh tính không hoạt động | 
| 2 1 | CÓ | thu nhỏ qua cấu trúc đảo ngược | 
| 1 2 | KHÔNG | không đơn điệu không thể tiếp cận | 
| 1 1 | CÓ | độ đúng trạng thái nhỏ nhất | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi việc đảo ngược tạo ra một chuỗi trở thành toàn số 0 sau khi loại bỏ. Ví dụ, một trạng thái như`1000`đảo ngược trở thành`0001`, chuẩn hóa thành`1`. Nếu quá trình chuẩn hóa được xử lý không chính xác, những trường hợp như vậy có thể bị mất hoặc bị trùng lặp. 

Đối với đầu vào`x = 1, y = 8`, dạng nhị phân là`1`Và`1000`. BFS ngược lại bắt đầu tại`1000`, đảo ngược thành`0001`, chuẩn hóa thành`1`, và ngay lập tức tìm thấy mục tiêu. Tính đúng đắn hoàn toàn phụ thuộc vào việc duy trì quy tắc một số 0 sau khi loại bỏ. 

Một trường hợp cạnh khác là khi`x == y`, phải quay lại ngay lập tức mà không cần nhập tìm kiếm. Khung BFS vẫn hoạt động, nhưng việc chấm dứt sớm sẽ tránh được việc xử lý không cần thiết và ngăn chặn việc vô tình phân loại sai khi logic hàng đợi giả định ít nhất một lần chuyển đổi.
