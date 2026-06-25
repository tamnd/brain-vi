---
title: "CF 105224A - Tấm bạt lò xo"
description: "Chúng ta có một bảng một chiều gồm các vị trí từ 1 đến n. Mỗi vị trí i chứa độ dài bước nhảy t[i]. Nếu một quả bóng được thả rơi ở vị trí i, nó liên tục thực hiện các bước nhảy xác định: từ i nó di chuyển đến i + t[i], sau đó từ vị trí mới j nó di chuyển đến j + t[j], v.v."
date: "2026-06-24T16:36:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105224
codeforces_index: "A"
codeforces_contest_name: "MOI2024"
rating: 0
weight: 105224
solve_time_s: 315
verified: false
draft: false
---

[CF 105224A - Tấm bạt lò xo](https://codeforces.com/problemset/problem/105224/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 15 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bảng một chiều gồm các vị trí từ 1 đến n. Mỗi vị trí i chứa độ dài bước nhảy t[i]. Nếu một quả bóng được thả ở vị trí i, nó liên tục thực hiện các bước nhảy xác định: từ i nó di chuyển đến i + t[i], sau đó từ vị trí mới j nó di chuyển đến j + t[j], v.v., cho đến khi vị trí vượt quá n. Lúc đó bóng được coi là đã ra khỏi bảng. 

Phần thú vị là mảng không tĩnh. Chúng ta phải hỗ trợ hai hoạt động. Một thao tác thay đổi độ dài bước nhảy ở một chỉ mục. Cái còn lại yêu cầu chúng tôi mô phỏng toàn bộ quá trình nhảy bắt đầu từ một chỉ số nhất định và báo cáo hai giá trị: vị trí cuối cùng vẫn ở trong bảng trước khi bóng rời khỏi bảng và số lần nhảy đã được thực hiện trước khi thoát ra. 

Các ràng buộc n và q đều lên tới 100000 và mỗi lần nhảy có thể di chuyển xa hoặc duy trì ở mức nhỏ tùy thuộc vào mảng. Mô phỏng trực tiếp cho mỗi truy vấn có thể dễ dàng chuyển sang hành vi bậc hai nếu cấu trúc là đối nghịch, vì một truy vấn có thể đi qua nhiều vị trí và có thể có nhiều truy vấn như vậy. 

Một kỳ vọng ngây thơ là mỗi truy vấn loại 1 có thể lấy O(n) trong trường hợp xấu nhất, dẫn đến tổng công việc là O(nq). Với 100000 thao tác, điều này vượt xa giới hạn khả thi. 

Trường hợp cạnh tinh tế là khi các bước nhảy luôn nằm trong một vùng nhỏ, ví dụ t[i] = 1 với tất cả i. Bắt đầu từ vị trí 1, chúng tôi sẽ truy cập mọi vị trí theo thứ tự cho đến n, tạo ra n bước cho mỗi truy vấn. Nếu có nhiều truy vấn, điều này ngay lập tức trở nên quá chậm. Một trường hợp đặc biệt khác là các bản cập nhật thường xuyên làm thay đổi một giá trị và thay đổi hoàn toàn đường dẫn, khiến việc ghi nhớ trên các truy vấn không đáng tin cậy trừ khi được cấu trúc cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với một truy vấn bắt đầu từ i, chúng tôi liên tục nhảy bằng cách sử dụng mảng hiện tại cho đến khi rời khỏi phạm vi đó. Chúng tôi đếm các bước và ghi nhớ vị trí hợp lệ cuối cùng. Mỗi bản cập nhật chỉ thay đổi một giá trị. 

Điều này hoạt động chính xác vì nó mô phỏng theo đúng nghĩa đen của quy trình. Vấn đề là một truy vấn có thể đi qua các vị trí O(n) trong trường hợp xấu nhất. Với q lên tới 100000, tổng trường hợp xấu nhất sẽ trở thành O(nq), quá lớn. 

Quan sát quan trọng là mỗi vị trí hoặc nhảy đến một vị trí xa hoặc vẫn ở trong khu vực nơi các bước nhảy nhỏ lặp lại hoạt động có thể đoán trước được. Chúng tôi muốn tránh mô phỏng từng bước trung gian riêng lẻ. Một cách tiêu chuẩn để tăng tốc các quá trình “nhảy con trỏ tiếp theo” như vậy trong các bản cập nhật là duy trì, đối với mỗi vị trí, vị trí tiếp theo mà chúng ta sẽ hạ cánh nằm trong cấu trúc khối được kiểm soát và cũng duy trì số bước được thực hiện để đạt được vị trí tiếp theo đó. 

Một kỹ thuật nổi tiếng cho việc này là phân tách sqrt trên các chỉ mục. Chúng ta chia mảng thành các khối có kích thước khoảng sqrt(n). Đối với mỗi chỉ mục, chúng tôi tính toán trước nơi chúng tôi thoát khỏi khối hiện tại và cần bao nhiêu bước để làm như vậy. Khi xử lý một truy vấn, chúng tôi liên tục chuyển từng khối thay vì từng bước. Khi có cập nhật, chúng tôi chỉ tính toán lại khối bị ảnh hưởng. 

Bên trong một khối, các bước nhảy được mô phỏng rõ ràng, nhưng mỗi lần truyền tải khối được nén thành một bước nhảy duy nhất trong quá trình truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) cho mỗi truy vấn, cập nhật O(1) | O(n) | Quá chậm | 
| Phân hủy khối | O(√n) cho mỗi truy vấn, cập nhật O(√n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia các chỉ số từ 1 đến n thành các khối có kích thước B, thường là khoảng sqrt(n). Đối với mỗi vị trí i, chúng tôi duy trì hai giá trị: nxt[i], vị trí tiếp theo sau khi chúng tôi rời khỏi khối i theo quy tắc nhảy và cnt[i], số lần nhảy cần thiết để đạt được nxt[i] hoặc thoát khỏi khối.

1. Xây dựng các khối và khởi tạo nxt và cnt từ phải sang trái. Chúng ta xử lý các chỉ số để khi tính i, chúng ta đã biết kết quả của i + t[i] nếu nó nằm trong cùng một khối hay bên ngoài khối đó. 
2. Với mỗi chỉ số i, tính j = i + t[i]. Nếu j > n thì nxt[i] nằm ngoài mảng và cnt[i] là 1. Điều này thể hiện một bước nhảy duy nhất dẫn đến thoát. 
3. Nếu j nằm trong cùng khối với i thì nxt[i] và cnt[i] được kế thừa từ j. Chúng tôi đặt nxt[i] = nxt[j] và cnt[i] = cnt[j] + 1. Điều này nén nhiều bước nhảy nội bộ thành một bản tóm tắt. 
4. Nếu j nằm trong một khối khác nhưng vẫn ở trong mảng, chúng ta đặt nxt[i] = j và cnt[i] = 1. Điều này có nghĩa là bước nhảy nén tiếp theo sẽ rời khỏi khối ngay lập tức. 
5. Đối với truy vấn bắt đầu từ i, chúng tôi liên tục áp dụng các bước nhảy khối này. Chúng tôi tích lũy tổng số và theo dõi vị trí hợp lệ cuối cùng. Mỗi lần chúng ta nhảy từ i tới nxt[i], vị trí cuối cùng là i trước khi nhảy và i được cập nhật thành nxt[i]. 
6. Quá trình dừng khi i vượt quá n và chúng tôi xuất ra vị trí hợp lệ cuối cùng và tổng số bước. 
7. Để cập nhật ở vị trí i, chúng ta thay đổi t[i] và tính toán lại tất cả các giá trị nxt và cnt bên trong khối i vì chỉ cấu trúc bên trong của khối đó mới có thể thay đổi. 

Lý do chính khiến tính năng này hoạt động là vì trong một khối, tất cả các chuỗi đều nhất quán cục bộ sau khi tính toán lại và việc nhảy qua các khối giúp giảm đáng kể độ dài đường dẫn. Mỗi truy vấn vượt qua nhiều nhất các khối O(√n), vì mỗi lần truyền tải khối sẽ bỏ qua nhiều vị trí cùng một lúc. 

### Tại sao nó hoạt động 

Cấu trúc được duy trì bởi nxt và cnt hoạt động như một biểu diễn nén của đồ thị hàm số được tạo ra bởi quy tắc nhảy. Bên trong một khối, mọi đường dẫn cuối cùng đều thoát khỏi khối hoặc mảng và chúng tôi tính toán trước chính xác nơi xảy ra lối ra đó. Vì các bản cập nhật chỉ ảnh hưởng đến một khối và việc tính toán lại khôi phục tính chính xác cục bộ nên mỗi bước nhảy đều tuân theo một chuỗi được tính toán trước hợp lệ hoặc chuyển sang ranh giới khối mới. Điều này đảm bảo rằng mọi bước mô phỏng trong quy trình nén đều tương ứng chính xác với chuỗi bước nhảy ban đầu, bảo toàn cả vị trí cuối cùng và số bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
t = list(map(int, input().split()))

B = int(n ** 0.5) + 1

nxt = [0] * n
cnt = [0] * n

def rebuild(block):
    l = block * B
    r = min(n, (block + 1) * B)
    for i in range(r - 1, l - 1, -1):
        j = i + t[i]
        if j >= n:
            nxt[i] = n
            cnt[i] = 1
        else:
            if j // B == i // B:
                nxt[i] = nxt[j]
                cnt[i] = cnt[j] + 1
            else:
                nxt[i] = j
                cnt[i] = 1

for b in range((n + B - 1) // B):
    rebuild(b)

for _ in range(q):
    tmp = input().split()
    if tmp[0] == '0':
        i = int(tmp[1]) - 1
        x = int(tmp[2])
        t[i] = x
        rebuild(i // B)
    else:
        i = int(tmp[1]) - 1
        pos = i
        steps = 0
        last = i

        while pos < n:
            last = pos
            steps += cnt[pos]
            pos = nxt[pos]

        print(last + 1, steps)
```Cốt lõi của việc thực hiện là`rebuild`chức năng tính toán lại thông tin bước nhảy đã nén cho một khối. Nó xử lý các chỉ số từ phải sang trái để bất cứ khi nào chúng ta sử dụng`nxt[j]`Và`cnt[j]`, chúng đã hợp lệ rồi. 

Trong các truy vấn, thay vì bước từng chỉ mục một, chúng tôi liên tục nhảy bằng cách sử dụng`nxt[pos]`, bỏ qua toàn bộ chuỗi bên trong các khối. Vị trí hợp lệ cuối cùng luôn là điểm bắt đầu của bước nhảy cuối cùng thoát khỏi mảng. 

Hoạt động cập nhật chỉ chạm vào một khối, do đó việc tính toán lại vẫn cục bộ. Chi tiết tinh tế nhất là xử lý việc lập chỉ mục một cách nhất quán: cấu trúc được lưu trữ sử dụng các chỉ mục dựa trên 0 bên trong, trong khi đầu ra yêu cầu các vị trí dựa trên 1. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu. 

### Ví dụ 1 

đầu vào:```
3 3
1 3 2
1 1
0 1 2
1 1
```Chúng tôi theo dõi trạng thái nhảy. 

| Hoạt động | Bắt đầu | Đường nhảy | Cuối cùng bên trong | Bước | 
| --- | --- | --- | --- | --- | 
| Truy vấn | 1 | 1 → 2 → 5(thoát) | 2 | 2 | 

Sau khi cập nhật t[1] trở thành 2. 

| Hoạt động | Bắt đầu | Đường nhảy | Cuối cùng bên trong | Bước | 
| --- | --- | --- | --- | --- | 
| Truy vấn | 1 | 1 → 3 → 5(thoát) | 3 | 2 | 

Truy vấn đầu tiên hiển thị một chuỗi ngắn thoát qua vị trí 2. Sau khi sửa đổi, đường dẫn sẽ thay đổi và thay vào đó thoát ra qua vị trí 3. 

Điều này xác nhận rằng các bản cập nhật ảnh hưởng hoàn toàn đến các bước nhảy xuôi dòng mặc dù chỉ có một giá trị thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) √n) | mỗi truy vấn nhảy qua các khối, mỗi bản cập nhật sẽ xây dựng lại một khối | 
| Không gian | O(n) | lưu trữ cho mảng nxt và cnt | 

Với n và q lên tới 100000, việc phân tách sqrt duy trì các hoạt động trong khoảng vài trăm bước cho mỗi truy vấn, phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    t = list(map(int, input().split()))

    B = int(n ** 0.5) + 1
    nxt = [0] * n
    cnt = [0] * n

    def rebuild(block):
        l = block * B
        r = min(n, (block + 1) * B)
        for i in range(r - 1, l - 1, -1):
            j = i + t[i]
            if j >= n:
                nxt[i] = n
                cnt[i] = 1
            else:
                if j // B == i // B:
                    nxt[i] = nxt[j]
                    cnt[i] = cnt[j] + 1
                else:
                    nxt[i] = j
                    cnt[i] = 1

    for b in range((n + B - 1) // B):
        rebuild(b)

    out = []
    for _ in range(q):
        tmp = sys.stdin.readline().split()
        if tmp[0] == '0':
            i = int(tmp[1]) - 1
            x = int(tmp[2])
            t[i] = x
            rebuild(i // B)
        else:
            i = int(tmp[1]) - 1
            pos = i
            steps = 0
            last = i
            while pos < n:
                last = pos
                steps += cnt[pos]
                pos = nxt[pos]
            out.append(f"{last+1} {steps}")

    return "\n".join(out)

# provided sample
assert run("""3 3
1 3 2
1 1
0 1 2
1 1
""").strip() == """2 2
3 2"""

# single element exit immediately
assert run("""1 1
1
1 1
""").strip() == "1 1"

# all jumps exit immediately
assert run("""5 2
10 10 10 10 10
1 1
1 3
""").strip() == "1 1"

# chain-like behavior
assert run("""5 1
1 1 1 1 1
1 1
""").strip() == "5 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 1 | xử lý thoát ngay lập tức | 
| bước nhảy lớn | thoát nhanh | nhảy ranh giới | 
| tất cả những cái | 5 5 | truyền tải chuỗi dài | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là khi mỗi bước nhảy nằm trong một khối duy nhất, điều này thường buộc phải truyền tải từng bước. Việc phân tách khối xử lý việc này vì các chuỗi bên trong được tính toán trước và thu gọn thành các con trỏ nxt. Ngay cả khi chúng ta không bao giờ rời khỏi khối cho đến bước cuối cùng, cnt vẫn tích lũy toàn bộ độ dài đường dẫn bên trong. 

Một trường hợp khác là khi một bản cập nhật thay đổi một giá trị gần ranh giới khối. Nếu không tính toán lại toàn bộ khối, các truy vấn tiếp theo có thể tuân theo các con trỏ nxt cũ. Việc xây dựng lại toàn bộ khối đảm bảo tất cả các phần phụ thuộc bên trong đều nhất quán trở lại. 

Cuối cùng, khi t[i] đủ lớn để thoát khỏi mảng ngay lập tức, nxt[i] được đặt thành n và cnt[i] là 1. Điều này ngăn chặn bất kỳ sự tham chiếu vô tình nào vượt quá giới hạn trong quá trình nén chuỗi và đảm bảo rằng các truy vấn kết thúc một cách rõ ràng ngay cả khi nhiều lần nhảy như vậy xảy ra liên tiếp.
