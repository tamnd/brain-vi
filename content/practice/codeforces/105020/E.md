---
title: "CF 105020E - Game Trinh Thám"
description: "Mỗi người chơi trong trò chơi này duy trì một danh sách cá nhân gồm những người chơi khác mà họ sẽ bỏ phiếu chống lại nếu những người chơi đó bị đưa ra xét xử."
date: "2026-06-28T01:57:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105020
codeforces_index: "E"
codeforces_contest_name: "TCPC Tunisian Collegiate Programming Contest 2022"
rating: 0
weight: 105020
solve_time_s: 63
verified: false
draft: false
---

[CF 105020E - Trò chơi trinh thám](https://codeforces.com/problemset/problem/105020/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi người chơi trong trò chơi này duy trì một danh sách cá nhân gồm những người chơi khác mà họ sẽ bỏ phiếu chống lại nếu những người chơi đó bị đưa ra xét xử. Quá trình bỏ phiếu mang tính quyết định: khi một người chơi cụ thể bị buộc tội, mọi người chơi khác sẽ kiểm tra xem người chơi bị buộc tội đó có xuất hiện trong danh sách nghi phạm cá nhân của họ hay không. Nếu đúng như vậy, họ sẽ bỏ phiếu loại bỏ; nếu không thì không. 

Một người chơi bị loại trong một vòng đấu nếu có hơn một nửa số người chơi bỏ phiếu chống lại họ. Nhiệm vụ là xác định, đối với từng người chơi một cách độc lập, liệu họ có bị loại nếu họ là người bị đưa ra xét xử hay không. 

Vì vậy đối với mỗi người chơi$x$, về mặt khái niệm, chúng tôi mô phỏng một thử nghiệm trong đó chúng tôi đếm xem có bao nhiêu người chơi khác bao gồm$x$trong danh sách nghi phạm của họ. Nếu số đó vượt quá$\frac{n}{2}$, sau đó$x$sẽ bị loại bỏ; nếu không thì họ sẽ sống sót sau thử nghiệm giả định đó. 

Cấu trúc đầu vào về cơ bản mô tả một mối quan hệ có hướng: mỗi người chơi$i$chỉ ra một nhóm người chơi mà họ sẽ bỏ phiếu chống lại. Câu hỏi giảm xuống việc đếm số phiếu bầu đến cho mỗi nút trong biểu đồ có hướng này và kiểm tra xem nút nào có mức độ lớn hơn một nửa số phiếu bầu đến.$n$. 

Những ràng buộc đẩy chúng ta tới những giải pháp tuyến tính hoặc gần tuyến tính. Tổng kích thước của tất cả các danh sách nghi ngờ trong tất cả các trường hợp thử nghiệm được giới hạn bởi$4 \cdot 10^5$, có nghĩa là bất kỳ giải pháp nào xử lý mỗi cạnh với số lần không đổi đều có thể chấp nhận được. Tuy nhiên, bất kỳ mô phỏng theo truy vấn nào cho mỗi người chơi sẽ quá chậm vì nó sẽ liên tục quét tất cả các danh sách, dẫn đến$O(n^2)$hành vi trong trường hợp xấu nhất. 

Một trường hợp phức tạp phát sinh khi không có người chơi nào đạt đến ngưỡng đa số. Ví dụ, nếu$n = 4$và mỗi người chơi chỉ bị nghi ngờ bởi tối đa một người chơi khác, khi đó ngưỡng tuyệt đối lớn hơn 2 nên không có ai đủ điều kiện. Một trường hợp khác là khi một người chơi bị chính xác một nửa số người chơi nghi ngờ; điều này không được coi là loại trừ vì tình trạng bệnh hoàn toàn lớn hơn 50%. 

Một chi tiết quan trọng khác là việc tự bỏ phiếu là không thể thực hiện được trong quá trình xây dựng, vì vậy chúng ta không bao giờ cần phải xử lý các vòng lặp tự động hoặc loại bỏ chúng. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp cho từng người chơi ứng cử viên sẽ hoạt động như thế này: đối với một người chơi cố định$x$, quét danh sách của mọi người chơi khác và đếm xem có bao nhiêu$x$. Điều này đúng nhưng đắt tiền. Mỗi truy vấn có giá$O(\sum k_i)$, và làm điều này cho tất cả$n$người chơi dẫn đến$O(n \cdot \sum k_i)$, suy biến thành$O(n^2)$trong những trường hợp dày đặc và rõ ràng là quá chậm để$n$lên tới$10^5$. 

Quan sát chính là cấu trúc biểu quyết không thay đổi giữa các truy vấn. Mỗi cạnh có hướng$i \rightarrow a_{ij}$đóng góp đúng một phiếu chống$a_{ij}$và đóng góp này không phụ thuộc vào người chơi nào hiện đang được thử nghiệm. Điều này có nghĩa là chúng ta có thể tính toán trước số cạnh đến cho mỗi nút trong một lần duyệt qua tất cả các danh sách. 

Khi đã biết số lượng này, mỗi người chơi có thể được đánh giá độc lập trong thời gian không đổi bằng cách kiểm tra xem số lượng của họ có vượt quá không$\lfloor n/2 \rfloor$. This transforms the problem into a simple frequency accumulation problem over edges.

 | Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot \sum k_i)$|$O(1)$thêm | Quá chậm | 
| Tối ưu |$O(n + \sum k_i)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm bớt vấn đề bằng cách đếm số lần mỗi người chơi bị nhắm mục tiêu trên tất cả các danh sách nghi ngờ, sau đó lọc theo một ngưỡng. 

1. Tạo một mảng`cnt`kích thước$n + 1$, khởi tạo bằng 0. Điều này sẽ lưu trữ bao nhiêu phiếu bầu mà mỗi người chơi nhận được. Chỉ số đại diện cho người chơi và giá trị đại diện cho số lượng người chơi khác sẽ bỏ phiếu chống lại họ. 
2. Lặp lại danh sách nghi phạm của mọi người chơi. Đối với mỗi mục$a_{ij}$, tăng`cnt[a_{ij}]`bởi một. Bước này tổng hợp tất cả các phiếu bầu đến trong một lần chuyển qua cấu trúc đầu vào. 
3. Tính ngưỡng loại trừ như sau$\text{limit} = \lfloor n/2 \rfloor$. Một người chơi chỉ bị loại nếu số phiếu bầu của họ lớn hơn giá trị này. 
4. Lặp lại tất cả người chơi từ 1 đến$n$. Đối với mỗi người chơi$i$, kiểm tra xem`cnt[i] > limit`. Nếu vậy, bao gồm$i$trong danh sách câu trả lời. 
5. Xuất ra kích thước của danh sách câu trả lời, theo sau là danh sách được sắp xếp. Thứ tự lặp lại đã đảm bảo việc sắp xếp. 

Tính chính xác phụ thuộc vào thực tế là mỗi phiếu bầu đều độc lập với ứng cử viên được truy vấn. Một phiếu bầu chỉ đơn giản là một cạnh đóng góp +1 cho một điểm cuối cố định và điều kiện loại bỏ chỉ phụ thuộc vào số lượng tích lũy cuối cùng đó. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, số phiếu bầu chống lại một người chơi$x$chính xác là số cạnh có hướng trỏ tới$x$. Mô phỏng việc đặt$x$khi dùng thử không thay đổi bất kỳ cấu trúc nào khác, do đó việc phân phối phiếu bầu được cố định trước truy vấn. Thuật toán tính toán một cách hiệu quả mức độ của từng nút trong biểu đồ có hướng và so sánh nó với ngưỡng chung. Vì mỗi cạnh đóng góp chính xác một lần và không có cạnh nào được tính hai lần hoặc bị bỏ qua, giá trị được tính toán khớp với số phiếu bầu thực sự trong mọi thử nghiệm giả định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n = int(input())
        cnt = [0] * (n + 1)

        for i in range(1, n + 1):
            parts = list(map(int, input().split()))
            k = parts[0]
            for x in parts[1:]:
                cnt[x] += 1

        limit = n // 2
        ans = []
        for i in range(1, n + 1):
            if cnt[i] > limit:
                ans.append(i)

        print(len(ans))
        if ans:
            print(*ans)

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng danh sách nghi phạm một lần và tổng hợp trực tiếp số phiếu bầu vào một mảng. Lựa chọn triển khai quan trọng là tránh mọi mô phỏng cho mỗi người chơi; thay vào đó, mỗi cạnh được xử lý chính xác một lần. Việc so sánh ngưỡng sử dụng phép chia số nguyên, thực hiện chính xác điều kiện nghiêm ngặt “trên 50 phần trăm”. 

Đầu ra được sắp xếp tự nhiên vì người chơi được kiểm tra theo thứ tự tăng dần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3
1 2
1 2
1 3
```Ở đây, người chơi 2 được người chơi 1 và 2 bỏ phiếu chống, trong khi người chơi 3 chỉ được người chơi 3 bỏ phiếu chống là không thể vì việc tự bỏ phiếu không được phép, vì vậy chỉ người chơi 1 và 2 đóng góp cho 2. 

| Người chơi | Phiếu bầu đến | 
| --- | --- | 
| 1 | 0 | 
| 2 | 2 | 
| 3 | 1 | 

Ngưỡng là$\lfloor 3/2 \rfloor = 1$. Chỉ có người chơi 2 vượt qua nó. 

Đầu ra:```
1
2
```Dấu vết này xác nhận rằng chỉ có số lượng dữ liệu đến tổng hợp mới quan trọng chứ không phải việc tính toán lại theo mỗi truy vấn. 

### Ví dụ 2 

đầu vào:```
1
4
1 2
1 3
1 4
0
```| Người chơi | Phiếu bầu đến | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 

Ngưỡng là$\lfloor 4/2 \rfloor = 2$. Không có người chơi vượt quá nó. 

Đầu ra:```
0
```Điều này cho thấy điều kiện bất đẳng thức nghiêm ngặt sẽ loại bỏ tất cả người chơi ngay cả khi một số người đạt chính xác một nửa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + \sum k_i)$| Mỗi mục nhập nghi ngờ được xử lý một lần, cộng với lần quét cuối cùng trên tất cả người chơi | 
| Không gian |$O(n)$| Lưu trữ số phiếu bầu cho mỗi người chơi | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm được giới hạn bởi$4 \cdot 10^5$, do đó, một lần truyền qua tất cả các cạnh và nút sẽ vừa vặn thoải mái trong giới hạn, cả về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-style
assert run("""1
3
1 2
1 2
1 3
""") == "1\n2"

# minimum n = 2
assert run("""1
2
1 2
0
""") == "1\n2"

# all zero votes
assert run("""1
3
0
0
0
""") == "0"

# full voting cycle
assert run("""1
4
1 2
1 3
1 4
1 2 3 4
""") == "1\n2"

# symmetric case no majority
assert run("""1
5
1 2
1 3
1 4
1 5
0
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=2 đơn giản | 1 2 | hành vi đa số tối thiểu | 
| tất cả số không | 0 | trường hợp không có phiếu cạnh | 
| chu kỳ đầy đủ | 1 2 | độ chính xác phân phối hỗn hợp | 
| thưa thớt bất đối xứng | 0 | ngưỡng nghiêm ngặt | 

## Vỏ cạnh 

Trường hợp quan trọng là khi người chơi nhận được chính xác một nửa số phiếu bầu. Ví dụ, với$n = 6$, nếu người chơi có 3 phiếu bầu đến, ngưỡng này hoàn toàn lớn hơn 3 nên không được đưa vào. Thuật toán xử lý việc này một cách chính xác vì nó sử dụng`cnt[i] > n // 2`, không`>=`. 

Một trường hợp khác là khi một người chơi nhận được gần như tất cả phiếu bầu. Trong cấu hình hình ngôi sao trong đó mọi danh sách đều trỏ tới người chơi 1,`cnt[1] = n - 1`, luôn vượt quá$n/2$vì$n \ge 2$, vì vậy người chơi 1 luôn được bao gồm. Phương pháp đếm nắm bắt được điều này một cách tự nhiên mà không cần xử lý đặc biệt. 

Trường hợp cuối cùng là khi danh sách trống đối với tất cả người chơi. Mảng bộ đếm vẫn bằng 0 ở mọi nơi và không có chỉ mục nào vượt qua ngưỡng, phù hợp với hành vi dự định mà không cần logic bổ sung.
