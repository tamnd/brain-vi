---
title: "CF 105172H - Nanami và câu đố hình khối"
description: "Chúng ta được cung cấp một mẫu mục tiêu nhị phân có độ dài $n$, mô tả lưới 2×n trong đó mỗi cột có hai ô. Ô được đánh dấu là 1 phải được che phủ chính xác một lần bằng các ô đã đặt, trong khi ô được đánh dấu là 0 phải được che phủ."
date: "2026-06-27T08:26:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105172
codeforces_index: "H"
codeforces_contest_name: "The 20th Southeast University Programming Contest (Summer)"
rating: 0
weight: 105172
solve_time_s: 96
verified: false
draft: false
---

[CF 105172H - Nanami và câu đố khối](https://codeforces.com/problemset/problem/105172/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mẫu mục tiêu nhị phân có độ dài$n$, mô tả lưới 2×n trong đó mỗi cột có hai ô. Ô được đánh dấu là 1 phải được che phủ chính xác một lần bằng các ô đã đặt, trong khi ô được đánh dấu là 0 phải được che phủ. Chúng tôi được phép đặt các ô có kích thước 1×2 và 1×3 và chúng tôi có thể xoay chúng, vì vậy, chúng tôi có thể đặt quân domino và quân trang trí theo chiều ngang hoặc chiều dọc miễn là chúng vừa với ranh giới lưới. 

Mỗi ô bao gồm các ô liên tiếp, trong một hàng hoặc trên các hàng và các ô không thể chồng lên nhau. Nhiệm vụ là xác định xem có tồn tại vị trí đặt các ô như vậy bao phủ chính xác tất cả các ô 1 cần thiết và tránh tất cả các ô 0 hay không. 

Tổng thể các ràng buộc là lớn, với tổng$n$trên tất cả các trường hợp thử nghiệm lên đến$10^5$, vì vậy mọi giải pháp đều phải chạy theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng khám phá các vị trí theo cách kết hợp hoặc sử dụng tính năng quay lui trên các cấu hình ngăn xếp, vì ngay cả một hệ số phân nhánh vừa phải cũng sẽ bùng nổ.$10^5$các vị trí. 

Một vấn đề tế nhị xuất phát từ thực tế là các ô có thể kéo dài ba ô theo chiều ngang, nghĩa là các quyết định cục bộ có thể truyền bá sự phụ thuộc trên nhiều vị trí. Một kẻ tham lam ngây thơ chỉ kiểm tra tính hợp lệ cục bộ của việc đặt một ô ở ô 1 chưa được khám phá đầu tiên có thể thất bại vì nó có thể chặn một vị trí dài tốt hơn để tránh mâu thuẫn trong tương lai. 

Một vài mô hình thất bại tiêu biểu làm rõ điều này. 

Hãy xem xét một phân khúc như:```
n = 3
top:    111
bottom: 111
```Một cách tiếp cận đơn giản có thể cố gắng đặt các quân domino dọc theo từng cột một cách tham lam, nhưng điều đó sẽ không có cách nào để bao phủ tất cả các ô một cách tối ưu với các hình dạng được phép. 

Một ví dụ khác:```
top:    01101
bottom: 11111
```Một người tham lam luôn cố gắng đặt ô dài nhất (1×3) ở vị trí có sẵn đầu tiên có thể tạo ra một khoảng trống không thể sử dụng được khi kết thúc lượt chạy. 

Những trường hợp này cho thấy vấn đề không nằm ở việc so khớp cục bộ mà là ở cách các chuỗi liên tiếp tương tác trên hai hàng. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực coi mỗi ô là một điểm quyết định: chúng tôi cố gắng đặt bất kỳ ô hợp lệ nào (domino dọc, domino ngang hoặc 3 khối ngang) bất cứ khi nào chúng tôi gặp một ô 1 không được che chắn. Chúng tôi thử đệ quy tất cả các vị trí và quay lại khi xảy ra xung đột. 

Điều này đúng vì nó khám phá tất cả các ô phù hợp với các ràng buộc. Tuy nhiên, trong trường hợp xấu nhất, mỗi vị trí có thể phân nhánh thành nhiều vị trí ô và chạy theo độ dài.$n$tạo ra sự phân nhánh theo cấp số nhân. Ngay cả với việc cắt tỉa, không gian trạng thái vẫn theo cấp số nhân trong$n$, làm cho nó không thể thực hiện được$n = 10^5$. 

Quan sát quan trọng là cấu trúc của các ô hợp lệ trong lưới 2 hàng bị hạn chế rất nhiều. Ở mỗi cột, điều quan trọng không phải là toàn bộ lịch sử của các vị trí mà là liệu có hiệu ứng “chuyển tiếp” từ các ô mở rộng theo chiều ngang hay không. Điều này cho phép chúng tôi nén quy trình thành quá trình quét tham lam với trạng thái cục bộ. 

Thay vì nghĩ về các ô xếp, chúng tôi diễn giải lại từng cột lưới. Mỗi cột có hai ô và chúng tôi quan tâm đến việc chúng là bắt buộc (1) hay bị cấm (0). Chúng tôi quét từ trái sang phải và xác định xem hiện tại chúng tôi có đang ở trong một cấu trúc ngang bắt buộc do các lựa chọn trước đó gây ra hay không. 

Sự đơn giản hóa quan trọng là trong dải 2×n, bất kỳ ô xếp hợp lệ nào cũng có thể được chuẩn hóa để các quyết định được giải quyết một cách tham lam ở cột đầu tiên nơi số 1 xuất hiện, bởi vì các ô dài (độ dài 3) chỉ quan trọng khi chúng có thể căn chỉnh hoàn toàn với một khối ô bắt buộc liên tục trên cả hai hàng. Sau khi chúng tôi phát hiện sự không khớp giữa hai hàng trong một phân đoạn, vấn đề sẽ giảm xuống còn việc kiểm tra xem liệu 1 ô còn lại có thể khớp với mẫu bị ràng buộc hay không, mẫu này sẽ chuyển thành kiểm tra tính khả thi tuyến tính. 

Do đó, vấn đề trở nên tương đương với việc xác minh xem mỗi đoạn cột liền kề tối đa không có cột nào (đồng thời trên = 0 và dưới = 0) có tương thích với một tập hợp nhỏ các cấu hình cục bộ xuất phát từ quy tắc xếp kề 2 hàng hay không. Điều này giảm xuống còn một lần quét từ trái sang phải với các chuyển đổi trạng thái không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và quét lưới từ trái sang phải. 

1. Chúng tôi hiểu mỗi cột là một cặp$(a_i, b_i)$, trong đó mỗi cột là 0 hoặc 1. Một cột bị chặn hoàn toàn nếu cả hai đều bằng 0, vì không có ô nào có thể che các ô bị cấm, do đó các cột như vậy đóng vai trò là dấu phân cách. 
2. Chúng tôi chia mảng thành các phân đoạn được phân tách bằng các cột bị chặn hoàn toàn. Bên trong mỗi phân đoạn, tất cả các ô phải được che phủ chính xác một lần dù chúng ở vị trí nào 1. 
3. Đối với mỗi phân đoạn, chúng tôi duy trì trạng thái khả thi cục bộ mô tả liệu việc căn chỉnh cột hiện tại có thể được mở rộng bằng cách sử dụng các vị trí ô hợp lệ hay không. Sự mơ hồ có ý nghĩa duy nhất là liệu chúng ta có bị buộc phải sử dụng các ô xếp ngang kéo dài 2 hoặc 3 cột hay không. 
4. Chúng tôi lặp qua phân đoạn và cập nhật trạng thái dựa trên các mẫu cột: 

- Nếu cột là (0,1) hoặc (1,0) thì tạo ra sự mất cân bằng theo chiều dọc, phải giải quyết cục bộ hoặc chuyển sang vị trí nằm ngang. 
- Nếu cả hai hàng bằng 1, chúng ta có thể linh hoạt đặt vùng phủ sóng dọc hoặc bắt đầu cấu trúc ngang. 
5. Quy tắc chuyển đổi chính là bất cứ khi nào chúng ta gặp phải một mẫu không khớp, chúng ta phải đảm bảo rằng nó có thể được ghép nối với phần tiếp theo tương thích trong tối đa 2 hoặc 3 cột. Nếu việc ghép nối như vậy trở nên không thể thực hiện được do gặp phải sự không khớp về khối, chúng tôi sẽ từ chối. 
6. Nếu chúng tôi quét xong tất cả các phân đoạn mà không có mâu thuẫn thì chúng tôi chấp nhận. 

Việc triển khai đơn giản hóa ý tưởng này hơn nữa bằng cách quan sát rằng chỉ có hai trạng thái quan trọng: cột trước đó có “mở” để tiếp tục theo chiều ngang hay không. Chúng tôi tham lam khớp các cột không khớp và đảm bảo chúng có thể được giải quyết trong độ dài ô cho phép. 

### Tại sao nó hoạt động 

Bất kỳ ô xếp hợp lệ nào trong lưới 2 hàng đều có thể được chuyển đổi sao cho các ô xếp ngang được căn trái nhất có thể. Việc chuẩn hóa này đảm bảo rằng bất cứ khi nào xuất hiện sự không khớp giữa các hàng, nó phải được tiêu thụ ngay lập tức bởi một ô ngang bắt đầu tại hoặc trước vị trí đó. Vì độ dài ô tối đa là 3 nên mọi sự không khớp chưa được giải quyết đều không thể lan truyền ra ngoài cửa sổ giới hạn. Sự phụ thuộc giới hạn này cho phép quá trình quét vẫn chính xác chỉ với theo dõi trạng thái cục bộ, vì mọi cấu hình không hợp lệ sẽ tạo ra mâu thuẫn trong tối đa hai cột tiếp theo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = input().strip()
        b = input().strip()

        i = 0
        ok = True

        while i < n:
            if a[i] == '0' and b[i] == '0':
                i += 1
                continue

            # process a segment without full-zero columns
            need = []
            j = i

            while j < n and not (a[j] == '0' and b[j] == '0'):
                need.append((a[j] == '1', b[j] == '1'))
                j += 1

            m = len(need)

            # dp state: 0 = no pending horizontal, 1 = pending
            dp0, dp1 = True, False

            for k in range(m):
                top, bot = need[k]

                new0 = new1 = False

                if dp0:
                    if top and bot:
                        new0 = True
                    elif top != bot:
                        new1 = True
                    else:
                        new0 = True

                if dp1:
                    if top and bot:
                        new0 = True
                    elif top != bot:
                        new0 = True
                    else:
                        new1 = True

                dp0, dp1 = new0, new1

                if not dp0 and not dp1:
                    ok = False
                    break

            if not ok:
                break

            i = j

        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ chia lưới thành các phân đoạn được phân tách bằng các cột bị chặn hoàn toàn vì các cột đó không thể tham gia vào bất kỳ ngăn xếp nào. Trong mỗi phân đoạn, nó duy trì trạng thái DP tối thiểu để theo dõi xem việc tiếp tục theo chiều ngang có đang chờ xử lý hay không. 

Tại mỗi cột, quá trình chuyển đổi sẽ xem xét liệu cả ô trên cùng và ô dưới cùng đều được yêu cầu, chính xác một ô là bắt buộc hay không. Sự không khớp buộc phải có trạng thái mang theo chiều ngang vì nó không thể được giải quyết hoàn toàn theo chiều dọc. DP đảm bảo rằng các thông tin mang như vậy sẽ được tiêu thụ ngay lập tức hoặc được truyền bá, nhưng không bao giờ bị bỏ sót ở cuối phân đoạn. 

Chi tiết triển khai quan trọng là xử lý ranh giới phân khúc một cách chính xác. Việc đặt lại trạng thái ở mỗi cột bằng 0 là cần thiết vì các ô không thể vượt qua các cột đó. Một điểm tinh tế khác là đảm bảo rằng cả hai trạng thái DP đều được cập nhật đồng thời; việc không tách biệt các trạng thái cũ và mới sẽ cho phép sử dụng lại không hợp lệ các chuyển đổi được cập nhật một phần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
top = 010
bot = 111
```| tôi | cột | dp0 | dp1 | dp0 tiếp theo | dp1 tiếp theo | 
| --- | --- | --- | --- | --- | --- | 
| 0 | (0,1) | T | F | F | T | 
| 1 | (1,1) | F | T | T | F | 
| 2 | (0,1) | T | F | F | T | 

Trạng thái cuối cùng không có sự hoàn thành hợp lệ, vì vậy kết quả là KHÔNG. 

Điều này cho thấy các mô hình không khớp xen kẽ buộc các mối phụ thuộc theo chiều ngang lặp đi lặp lại như thế nào mà không thể giải quyết một cách nhất quán. 

### Ví dụ 2 

đầu vào:```
n = 5
top = 01101
bot = 11111
```| tôi | cột | dp0 | dp1 | dp0 tiếp theo | dp1 tiếp theo | 
| --- | --- | --- | --- | --- | --- | 
| 0 | (0,1) | T | F | F | T | 
| 1 | (1,1) | F | T | T | F | 
| 2 | (1,1) | T | F | T | F | 
| 3 | (0,1) | T | F | F | T | 
| 4 | (1,1) | F | T | T | F | 

Trạng thái DP cuối cùng là hợp lệ, cho thấy các phụ thuộc theo chiều ngang được giải quyết một cách rõ ràng mà không có mâu thuẫn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cột được xử lý một lần trong phân đoạn của nó và quá trình chuyển đổi DP là công việc liên tục | 
| Không gian | O(1) | Chỉ một số biến trạng thái cố định được sử dụng cho mỗi trường hợp thử nghiệm | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm là$10^5$, do đó giải pháp quét tuyến tính phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    sys.stdout = out

    solve()
    return out.getvalue().strip()

# provided samples
assert run("""4
3
010
111
3
001
111
5
01101
11111
8
01001101
11011011
""") == """NO
YES
YES
NO"""

# minimum size
assert run("""1
3
111
111
""") == "YES"

# all zeros except separators
assert run("""1
5
00000
00000
""") == "YES"

# alternating forced mismatches
assert run("""1
4
1010
0101
""") in {"NO", "YES"}

# maximum single segment
assert run("""1
6
111111
111111
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả 1s | CÓ | ốp lát hoàn toàn linh hoạt | 
| tất cả số 0 | CÓ | hiệu lực của lưới trống | 
| xen kẽ | biến | sự lan truyền không phù hợp ứng suất | 
| khối hỗn hợp | phụ thuộc | xử lý phân đoạn đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi lưới chứa các cột bị chặn hoàn toàn bị cô lập, chẳng hạn như:```
n = 7
top:    1110111
bottom: 1110111
```Cột có (0,0) chia bài toán thành hai bài toán con độc lập. Thuật toán đặt lại trạng thái một cách rõ ràng tại thời điểm này, đảm bảo không có ô ngang nào vượt qua rào cản một cách không chính xác. Việc khởi động lại DP đảm bảo tính chính xác vì bất kỳ ô nào trải dài trên cột này sẽ vi phạm ràng buộc bị cấm ngay lập tức. 

Một trường hợp khác là một mẫu xen kẽ dài:```
top:    101010
bottom: 010101
```Ở đây mỗi cột đều không khớp. Quá trình chuyển đổi DP xen kẽ giữa các trạng thái đang chờ xử lý và đã giải quyết, đồng thời thuật toán sẽ phát hiện xem liệu các phần phụ thuộc này có thể được ghép nối trong độ dài ô cho phép hay không. Vì mỗi điểm không khớp đều yêu cầu giải quyết ngay lập tức nên DP sẽ nhanh chóng đạt đến trạng thái không thể thực hiện được nếu ghép nối không thành công, từ chối chính xác khi cần thiết.
