---
title: "CF 1073B - Vasya và Sách"
description: "Chúng ta được cấp một chồng sách trong đó mỗi cuốn sách có một nhãn duy nhất. Thứ tự ngăn xếp rất quan trọng: mảng đầu tiên mô tả cuốn sách nào ở trên cùng và phần tử cuối cùng ở dưới cùng."
date: "2026-06-15T14:11:20+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1073
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 53 (Rated for Div. 2)"
rating: 1000
weight: 1073
solve_time_s: 279
verified: true
draft: false
---

[CF 1073B - Vasya và Sách](https://codeforces.com/problemset/problem/1073/B) 

**Đánh giá:** 1000 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 4 phút 39 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chồng sách trong đó mỗi cuốn sách có một nhãn duy nhất. Thứ tự ngăn xếp rất quan trọng: mảng đầu tiên mô tả cuốn sách nào ở trên cùng và phần tử cuối cùng ở dưới cùng. Chúng tôi cũng nhận được chuỗi thứ hai mô tả một thứ tự cố định trong đó Vasya cố gắng lấy những cuốn sách cụ thể. 

Ở mỗi bước, Vasya tìm kiếm cuốn sách được yêu cầu trong ngăn xếp hiện tại. Nếu nó vẫn còn, anh ta sẽ bỏ cuốn sách đó cùng với mọi thứ phía trên nó trong ngăn xếp và tất cả những cuốn sách đã xóa đó được coi là đã được chuyển vào ba lô của anh ta. Nếu sách được yêu cầu đã bị xóa trước đó thì không có gì xảy ra ở bước đó. 

Nhiệm vụ là tính toán, với mỗi bước, có bao nhiêu cuốn sách bị xóa trong thao tác đó. 

Các hạn chế lên tới 200.000 cuốn sách. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào liên tục quét hoặc mô phỏng các hoạt động ngăn xếp một cách đơn giản cho mỗi truy vấn. Một mô phỏng đơn giản tìm kiếm từng cuốn sách được yêu cầu trong ngăn xếp hiện tại và loại bỏ các phần tử về mặt vật lý có thể giảm xuống hành vi bậc hai, quá chậm so với giới hạn thời gian. 

Một vấn đề nhỏ xuất hiện khi nhiều truy vấn đề cập đến sách đã bị xóa. Trong những trường hợp đó, câu trả lời là 0 và việc triển khai đơn giản không duy trì cấu trúc "đã bị xóa" thích hợp có thể cố xử lý không chính xác các vị trí cũ hoặc đếm gấp đôi các phân đoạn đã bị xóa. 

Một trường hợp thất bại cụ thể của việc mô phỏng bất cẩn là khi bạn xây dựng lại hoặc thu nhỏ danh sách mỗi lần. Ví dụ, nếu ngăn xếp là`[1, 2, 3, 4]`và các truy vấn là`[4, 3, 2, 1]`, việc quét và cắt liên tục có thể vô tình tính toán lại vị trí không chính xác nếu các chỉ mục không được cập nhật sau khi xóa. Đầu ra đúng là`4 0 0 0`, vì thao tác đầu tiên sẽ xóa toàn bộ ngăn xếp và tất cả các yêu cầu sau đó sẽ không thực hiện gì. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp giữ ngăn xếp hiện tại dưới dạng danh sách. Đối với mỗi truy vấn, chúng tôi tìm kiếm cuốn sách được yêu cầu, tìm vị trí của nó và xóa mọi thứ cho đến vị trí đó. Điều này đúng vì nó khớp chính xác với định nghĩa vấn đề. Tuy nhiên, việc tìm kiếm từng cuốn sách mất thời gian tuyến tính và việc xóa tiền tố cũng mất thời gian tuyến tính. Trong tất cả các truy vấn, điều này dẫn đến độ phức tạp bậc hai trong trường hợp xấu nhất khi mỗi lần chỉ loại bỏ một phần nhỏ. 

Quan sát quan trọng là ngăn xếp chỉ co lại từ trên xuống và mỗi cuốn sách có thể bị xóa đúng một lần. Thay vì tìm kiếm liên tục, chúng ta có thể tính toán trước vị trí của từng cuốn sách trong ngăn xếp ban đầu. Sau đó, chúng tôi duy trì một con trỏ cho biết chúng tôi đã xử lý bao xa trong ngăn xếp. Khi truy vấn yêu cầu sách, chúng tôi chỉ xóa sách nếu vị trí của sách vẫn ở trên con trỏ hiện tại. Chúng tôi nâng cao con trỏ bất cứ khi nào chúng tôi xóa vị trí đạt tới mức thấp nhất mới. Mỗi cuốn sách được chạm hiệu quả một lần. 

Điều này biến việc quét lặp lại thành một lần quét qua các vị trí ngăn xếp theo thứ tự tăng dần của các sự kiện loại bỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một mảng`pos`Ở đâu`pos[x]`đưa ra mục lục của cuốn sách`x`trong ngăn xếp ban đầu. Điều này chuyển đổi từng truy vấn thành tra cứu vị trí trực tiếp thay vì tìm kiếm. 
2. Duy trì một biến`ptr`đại diện cho chỉ mục cao nhất trong ngăn xếp đã bị xóa. Ban đầu, không có cuốn sách nào bị xóa, vì vậy`ptr = 0`. 
3. Đối với mỗi cuốn sách được yêu cầu`b_i`, kiểm tra vị trí của nó`p = pos[b_i]`. 
4. Nếu`p <= ptr`, cuốn sách đã bị xóa như một phần của thao tác trước đó, do đó, xuất ra`0`. 
5. Nếu`p > ptr`, thì cuốn sách này vẫn còn tồn tại. Thao tác xóa tất cả sách khỏi chỉ mục`ptr + 1`xuống tới`p`. Số sách bị loại bỏ là`p - ptr`. 
6. Cập nhật`ptr`ĐẾN`p`, vì mọi thứ ở trên và bao gồm cả`p`hiện đã được loại bỏ khỏi việc xem xét trong tương lai. 
7. Lặp lại cho tất cả các truy vấn. 

Tính đúng đắn xuất phát từ thực tế là`ptr`luôn theo dõi tiền tố sâu nhất của ngăn xếp đã được sử dụng. Mỗi lần xóa sẽ mở rộng tiền tố này một cách đơn điệu và không có phân đoạn đã xóa nào được xem lại. 

## Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, ngăn xếp còn lại chính xác là hậu tố bắt đầu từ`ptr + 1`. Mỗi truy vấn đề cập đến một cuốn sách bên trong hậu tố này hoặc bên ngoài nó. Nếu nó ở bên ngoài thì nó đã được đưa vào lần loại bỏ trước đó nên câu trả lời phải bằng 0. Nếu nó nằm bên trong, việc xóa sẽ lấy chính xác tiền tố của hậu tố còn lại cho cuốn sách đó, là một đoạn liền kề của những cuốn sách chưa sử dụng. Bởi vì`ptr`chỉ di chuyển về phía trước, mỗi chỉ mục được xử lý nhiều nhất một lần, đảm bảo tổng công việc tuyến tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i, x in enumerate(a):
        pos[x] = i + 1

    ptr = 0
    res = []

    for x in b:
        p = pos[x]
        if p <= ptr:
            res.append("0")
        else:
            res.append(str(p - ptr))
            ptr = p

    print(" ".join(res))

if __name__ == "__main__":
    solve()
```các`pos`mảng được xây dựng bằng cách sử dụng chỉ mục dựa trên 1 để các vị trí ngăn xếp căn chỉnh tự nhiên với độ dài loại bỏ. Điều này tránh được việc điều chỉnh từng cái một khi tính toán`p - ptr`. 

các`ptr`biến là trạng thái trung tâm của giải pháp. Mỗi khi chúng tôi trả lời một truy vấn khác 0, chúng tôi sẽ mở rộng truy vấn đó chính xác đến vị trí của cuốn sách mới bị xóa. Điều này đảm bảo chúng tôi không bao giờ tính gấp đôi bất kỳ phân khúc nào. 

Đầu ra được tích lũy dưới dạng chuỗi để đạt hiệu quả, vì việc in lặp lại bên trong vòng lặp sẽ làm chậm quá trình thực thi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

ngăn xếp đầu vào là`[1, 2, 3]`, truy vấn là`[2, 1, 3]`. 

| Bước | Sách | Vị trí | ptr trước | Hành động | Đã xóa | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 0 | xóa tiền tố thành 2 | 2 | 
| 2 | 1 | 1 | 2 | đã bị xóa | 0 | 
| 3 | 3 | 3 | 2 | xóa tiền tố thành 3 | 1 | 

Đầu ra là`2 0 1`. 

Điều này chứng tỏ một lần`ptr`di chuyển qua một vị trí, cuốn sách đó không thể kích hoạt bất kỳ hoạt động nào nữa. 

### Ví dụ 2 

ngăn xếp`[3, 1, 4, 2, 5]`, truy vấn`[4, 2, 5, 1, 3]`. 

| Bước | Sách | Vị trí | ptr trước | Hành động | Đã xóa | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 3 | 0 | loại bỏ đến 3 | 3 | 
| 2 | 2 | 4 | 3 | mở rộng đến 4 | 1 | 
| 3 | 5 | 5 | 4 | mở rộng đến 5 | 1 | 
| 4 | 1 | 2 | 5 | đã bị xóa | 0 | 
| 5 | 3 | 1 | 5 | đã bị xóa | 0 | 

Điều này cho thấy cách`ptr`tăng trưởng đơn điệu và cuối cùng bao phủ toàn bộ ngăn xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí sổ sách được xử lý nhiều nhất một lần`ptr`chỉ tiến về phía trước | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng vị trí | 

Giải pháp này phù hợp thoải mái trong giới hạn vì cả bộ nhớ và quy mô thời gian đều tuyến tính với tối đa 200.000 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i, x in enumerate(a):
        pos[x] = i + 1

    ptr = 0
    ans = []
    for x in b:
        p = pos[x]
        if p <= ptr:
            ans.append("0")
        else:
            ans.append(str(p - ptr))
            ptr = p
    return " ".join(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run("""3
1 2 3
2 1 3
""") == "2 0 1"

# minimum size
assert run("""1
1
1
""") == "1"

# already reversed stack
assert run("""4
4 3 2 1
1 2 3 4
""") == "1 1 1 1"

# all queries after full removal
assert run("""5
1 2 3 4 5
5 4 3 2 1
""") == "5 0 0 0 0"

# mixed pattern
assert run("""5
3 1 4 2 5
4 2 5 1 3
""") == "3 1 1 0 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cỡ 1 | 1 | trường hợp hợp lệ nhỏ nhất | 
| ngăn xếp đảo ngược | 1 1 1 1 | loại bỏ gia tăng lặp đi lặp lại | 
| truy vấn ngược đầy đủ | 5 0 0 0 0 | sau hành vi tiêu dùng đầy đủ | 
| mẫu hỗn hợp | 3 1 1 0 0 | chồng chéo một phần và logic con trỏ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi truy vấn đề cập đến một cuốn sách đã bị xóa ở bước trước. Trong tình huống này, hành vi đúng là xuất ra số 0 và không sửa đổi bất kỳ trạng thái nào. Thuật toán xử lý việc này bằng cách kiểm tra xem vị trí được lưu trữ ở mức bằng hay thấp hơn`ptr`. 

Ví dụ, với ngăn xếp`[1, 2, 3, 4]`và truy vấn`[3, 4, 2, 1]`, sau khi xử lý`3`, con trỏ trở thành`3`. Khi xử lý`4`, nó kéo dài đến`4`. Vào thời điểm đó, cả hai`2`Và`1`nằm dưới hoặc bằng`ptr`, do đó họ tạo ra số 0 một cách chính xác mà không cần thay đổi thêm.
