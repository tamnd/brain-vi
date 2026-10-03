---
title: "CF 104882E - Đồng bộ hóa hiệu quả"
description: "Chúng tôi đang duy trì một bộ sưu tập k phiên bản độc lập của cùng một mảng có kích thước n. Ban đầu, tất cả k mảng đều giống hệt nhau và bằng một mảng cơ sở nhất định. Theo thời gian, chúng tôi xử lý hai loại hoạt động được đánh dấu thời gian."
date: "2026-06-28T09:18:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104882
codeforces_index: "E"
codeforces_contest_name: "Voronezh State University - Sitronics contest II"
rating: 0
weight: 104882
solve_time_s: 51
verified: true
draft: false
---

[CF 104882E - Đồng bộ hóa hiệu quả](https://codeforces.com/problemset/problem/104882/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một bộ sưu tập k phiên bản độc lập của cùng một mảng có kích thước n. Ban đầu, tất cả k mảng đều giống hệt nhau và bằng một mảng cơ sở nhất định. Theo thời gian, chúng tôi xử lý hai loại hoạt động được đánh dấu thời gian. 

Thao tác “Đặt” sửa đổi một vị trí trong một máy chủ tại một thời điểm cụ thể và thao tác “Nhận” truy vấn một giá trị từ một máy chủ tại một vị trí tại một thời điểm cụ thể. Điều khó khăn là tất cả các máy chủ đều đồng bộ hóa định kỳ mỗi s đơn vị thời gian và trong quá trình đồng bộ hóa, chúng lại trở nên giống hệt nhau. Khi họ điều chỉnh những khác biệt, mỗi vị trí mảng sẽ được giải quyết bằng cách chọn giá trị đến từ thời điểm sửa đổi gần đây nhất trên tất cả các máy chủ. 

Một cách hữu ích để nghĩ về điều này là mọi vị trí đều hoạt động giống như một hệ thống độc lập với quy tắc "ghi cuối cùng sẽ thắng", nhưng chỉ trong phạm vi chu kỳ đồng bộ hóa gần đây nhất. Đồng bộ hóa sẽ đặt lại hệ thống về trạng thái toàn cầu nhất quán một cách hiệu quả, hợp nhất tất cả các bản cập nhật đã diễn ra kể từ lần đồng bộ hóa trước đó. 

Các ràng buộc đẩy chúng tôi ra khỏi bất kỳ giải pháp nào liên quan đến tất cả các máy chủ hoặc quét các mảng cho mỗi truy vấn. Với tối đa 10^6 thao tác và mảng lên tới 10^5, mọi cách tiếp cận O(n) hoặc thậm chí O(k) trên mỗi thao tác đều không thể thực hiện được. Ngay cả việc bảo trì trạng thái trên mỗi máy chủ cũng quá tốn kém nếu chúng ta không cẩn thận, vì việc đồng bộ hóa đơn giản sẽ yêu cầu O(k·n) hoạt động trong mỗi s đơn vị thời gian, có thể là 10^10 thao tác trong trường hợp xấu nhất. 

Trường hợp cạnh tinh tế xuất phát từ thứ tự đồng bộ hóa. Nếu một yêu cầu đến chính xác vào thời điểm t trong đó t mod s = 0, thì việc đồng bộ hóa xảy ra trước tiên, sau đó yêu cầu sẽ được áp dụng. Điều này thay đổi ý nghĩa của “bản cập nhật nào thuộc về chu kỳ nào”. 

Một ví dụ nhỏ về vấn đề này là: 

đầu vào: 

k = 2, n = 2, s = 5 

a = [0, 0] 

5 Nhận 1 1 

Tại thời điểm thứ 5, quá trình đồng bộ hóa diễn ra trước truy vấn. Vì vậy, truy vấn sẽ nhìn thấy trạng thái sau khi hợp nhất các bản cập nhật đến thời điểm 5. Việc triển khai đơn giản xử lý các truy vấn trước tiên sẽ quan sát không chính xác trạng thái cũ. 

Một vấn đề tế nhị khác là các bản cập nhật trên các máy chủ khác nhau không tồn tại độc lập mãi mãi. Chúng có thể bị ghi đè bằng cách hợp nhất đồng bộ hóa sau này, do đó, chỉ lưu trữ các mảng trên mỗi máy chủ mà không theo dõi thứ tự thời gian trên toàn cầu sẽ làm mất tính chính xác. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng từng máy chủ một cách độc lập theo đúng nghĩa đen. Đối với mỗi Bộ, chúng tôi cập nhật một mảng của máy chủ. Đối với Get, chúng tôi đọc trực tiếp. Mỗi đơn vị thời gian, chúng tôi kích hoạt đồng bộ hóa, quét tất cả các vị trí trên tất cả các máy chủ và chọn dấu thời gian cập nhật gần đây nhất cho mỗi vị trí. 

Điều này đúng, nhưng việc đồng bộ hóa là một thảm họa. Mỗi lần đồng bộ hóa có chi phí O(k·n) và có thể có tối đa O(10^9/s) sự kiện đồng bộ hóa. Ngay cả khi s lớn, trường hợp xấu nhất vẫn xảy ra với khoảng 10^14 thao tác trong cài đặt suy biến. Vấn đề thực sự là việc đồng bộ hóa buộc phải tính toán lại toàn bộ từng ô trên tất cả các máy chủ. 

Quan sát chính là việc đồng bộ hóa mang tính toàn cầu nhưng độc lập về vị trí. Mỗi vị trí mảng phát triển độc lập với những vị trí khác. Đối với vị trí cố định i, chúng tôi chỉ quan tâm đến nhiệm vụ gần đây nhất giữa tất cả các máy chủ, nhưng chỉ trong phân đoạn đồng bộ hóa hiện tại. Điều này gợi ý rằng thay vì lưu trữ toàn bộ mảng trên mỗi máy chủ, chúng tôi chỉ cần theo dõi thời gian và giá trị ghi mới nhất trên mỗi vị trí, nhưng trạng thái đó sẽ đặt lại một cách hợp lý ở ranh giới đồng bộ hóa.

Thay vì đồng bộ hóa rõ ràng tất cả các máy chủ, chúng tôi diễn giải lại hệ thống dưới dạng dòng thời gian được chia thành các chu kỳ có độ dài s. Trong mỗi chu kỳ, các bản cập nhật sẽ ghi đè các bản cập nhật trước đó và tại ranh giới chu kỳ, trạng thái được hợp nhất thành “đường cơ sở” trở thành điểm bắt đầu cho chu kỳ tiếp theo. Sự giảm thiểu quan trọng là chúng ta không bao giờ cần phải mô phỏng các máy chủ một cách riêng biệt; chúng ta chỉ cần một cấu trúc toàn cục lưu trữ, đối với mỗi vị trí, lần ghi cuối cùng trong chu kỳ hiện tại cộng với giá trị đã cam kết từ các chu kỳ trước đó. 

Để thực hiện điều này hiệu quả, chúng tôi duy trì hai lớp trạng thái: một mảng toàn cầu đã cam kết biểu thị trạng thái được đồng bộ hóa cho đến chu kỳ đầy đủ cuối cùng và cấu trúc tạm thời cho các bản cập nhật lưu trữ chu kỳ hiện tại kể từ lần đồng bộ hóa cuối cùng. Khi đạt đến ranh giới đồng bộ hóa, chúng tôi sẽ chuyển các bản cập nhật tạm thời vào mảng đã cam kết bằng cách lấy dấu thời gian mới nhất cho mỗi vị trí. Vì phải hỗ trợ tối đa 10^6 truy vấn nên chúng tôi đảm bảo rằng mỗi vị trí chỉ được xử lý khi nó thực sự được sửa đổi, sử dụng từ điển lười thay vì quét tất cả n vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(q · k · n) | O(k · n) | Quá chậm | 
| Hợp nhất chu kỳ mỗi vị trí lười biếng | O(q log n) | O(n + cập nhật) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các sự kiện theo thứ tự thời gian tăng dần, theo dõi chu kỳ đồng bộ hóa hiện tại. 

1. Chúng tôi tính chỉ số chu kỳ hiện tại là t // s. Bất cứ khi nào chỉ số chu kỳ tăng lên, chúng tôi thực hiện bước đồng bộ hóa. Bước này hợp nhất tất cả các bản cập nhật đang chờ xử lý từ chu kỳ trước vào trạng thái đã cam kết. 
2. Chúng tôi duy trì một từ điển trên mỗi máy chủ (hoặc tương đương là một cấu trúc dùng chung được khóa theo vị trí máy chủ) lưu trữ thời gian và giá trị cập nhật cuối cùng trong chu kỳ hiện tại. Khi chúng tôi áp dụng Set sid pos val, chúng tôi lưu trữ (thời gian, giá trị) cho cặp đó, ghi đè mọi cập nhật trước đó trong cùng một chu kỳ. 
3. Đối với mỗi Get sid pos, chúng ta phải quyết định xem giá trị liên quan mới nhất là từ trạng thái đã cam kết hay từ chu kỳ hiện tại. Nếu có bản cập nhật đang chờ xử lý cho (sid, pos) đó trong chu kỳ hiện tại, chúng tôi sẽ so sánh dấu thời gian của nó với ranh giới đồng bộ hóa cuối cùng. Nếu nó mới hơn, chúng tôi sử dụng nó; nếu không chúng ta sẽ quay trở lại giá trị mảng đã cam kết. 
4. Trong quá trình đồng bộ hóa, chúng tôi chỉ lặp lại các khóa đã được sửa đổi trong chu kỳ hiện tại. Đối với mỗi (sid, pos), chúng tôi cập nhật giá trị cam kết tại pos nếu dấu thời gian được lưu trữ lớn hơn dấu thời gian cam kết hiện tại cho vị trí đó. Sau khi xử lý, chúng tôi xóa cấu trúc tạm thời. 
5. Sau khi xử lý tất cả các truy vấn, chúng tôi tiếp tục trả lời các thao tác Nhận bằng cách sử dụng cùng một quy tắc, đảm bảo rằng mỗi truy vấn đều có trạng thái nhất quán với tất cả các ranh giới đồng bộ hóa trước đó. 

Lý do điều này hoạt động là vì việc đồng bộ hóa chỉ phụ thuộc vào lần ghi gần đây nhất cho mỗi vị trí. Mọi cập nhật cũ hơn trong cùng một chu kỳ đều không liên quan và các cập nhật ngoài chu kỳ đã được thể hiện ở trạng thái đã cam kết. Do đó, mỗi vị trí chỉ cần nhớ một ứng viên tốt nhất trong mỗi chu kỳ cộng với một giá trị cam kết toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k, n, s = map(int, input().split())
    a = list(map(int, input().split()))
    q = int(input())

    committed_val = a[:]
    committed_time = [0] * n

    # pending updates in current cycle: (sid, pos) -> (time, val)
    pending = {}

    current_cycle = 0

    def sync():
        nonlocal pending
        # merge pending into committed
        for (sid, pos), (t, val) in pending.items():
            if t >= committed_time[pos]:
                committed_time[pos] = t
                committed_val[pos] = val
        pending = {}

    for _ in range(q):
        parts = input().split()
        t = int(parts[0])
        typ = parts[1]

        cycle = t // s
        if cycle != current_cycle:
            sync()
            current_cycle = cycle

        if typ == "Set":
            sid = int(parts[2]) - 1
            pos = int(parts[3]) - 1
            val = int(parts[4])
            pending[(sid, pos)] = (t, val)

        else:
            sid = int(parts[2]) - 1
            pos = int(parts[3]) - 1

            best = committed_val[pos]
            best_time = committed_time[pos]

            if (sid, pos) in pending:
                t2, v2 = pending[(sid, pos)]
                if t2 >= best_time:
                    best = v2

            print(best)

def main():
    solve()

if __name__ == "__main__":
    main()
```Việc triển khai tách thế giới thành trạng thái cam kết và trạng thái chu kỳ hiện tại. Các mảng đã cam kết lưu trữ ảnh chụp nhanh được đồng bộ hóa lần cuối cùng với dấu thời gian để chúng tôi có thể so sánh độ mới. Từ điển đang chờ xử lý chỉ lưu trữ các sửa đổi trong chu kỳ hiện tại, điều này rất cần thiết vì việc đồng bộ hóa chỉ quan tâm đến thứ tự tương đối trong một chu kỳ. 

Chuyển đổi chu kỳ được xử lý một cách lười biếng. Thay vì mô phỏng từng giây, chúng tôi phát hiện khi nào phép chia số nguyên t // s thay đổi. Tại thời điểm đó, chúng tôi chuyển các bản cập nhật đang chờ xử lý sang trạng thái đã cam kết. Điều này tránh bất kỳ vòng lặp định kỳ theo thời gian. 

Hoạt động Nhận sẽ kiểm tra trạng thái đang chờ xử lý trước tiên vì nó thể hiện các bản cập nhật chưa được đồng bộ hóa gần đây nhất. Nếu không có bản cập nhật đang chờ xử lý nào tồn tại hoặc nó cũ hơn trạng thái đã cam kết, chúng tôi sẽ trả về giá trị đã cam kết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

k = 2, n = 5, s = 10 

a = [3, 2, 2, 4, 4] 

Chúng tôi xử lý một tập hợp con các truy vấn: 

| thời gian | sự kiện | đang chờ xử lý | cam kết thay đổi | đầu ra | 
| --- | --- | --- | --- | --- | 
| 4 | Nhận (2,3) | {} | không | 2 | 
| 7 | Đặt (2,3)=1 | {(2,3)} | không | - | 
| 8 | Nhận (1,3) | {(2,3)} | không | 2 | 
| 9 | Nhận (2,3) | {(2,3)} | không | 1 | 
| 100 | Nhận (1,3) | sau khi đồng bộ hóa | cam kết cập nhật | 2 | 

Tại thời điểm thứ 10, quá trình đồng bộ hóa diễn ra nhưng do không có máy chủ nào khác sửa đổi vị trí 3 nên bản cập nhật đang chờ xử lý không bị bất kỳ đối thủ cạnh tranh nào ghi đè. Nó trở nên cam kết. 

Điều này cho thấy các bản cập nhật đang chờ xử lý chỉ quan trọng trong chu kỳ của chúng và chỉ được hợp nhất ở các ranh giới. 

### Ví dụ 2 

đầu vào: 

k = 5, n = 5, s = 60 

a = [0,0,0,0,0] 

| thời gian | sự kiện | chu kỳ | hành động | hiệu ứng trạng thái | 
| --- | --- | --- | --- | --- | 
| 1 | Nhận (1,5) | 0 | đọc cam kết | 0 | 
| 2 | Đặt (2,2)=7 | 0 | cửa hàng đang chờ xử lý | (2,2) đang chờ xử lý | 
| 7 | Đặt (5,2)=1 | 0 | ghi đè đang chờ xử lý | (5,2) đã thêm | 
| 59 | Nhận (2,2) | 0 | đọc đang chờ xử lý | 7 | 
| 60 | Nhận (2,2) | 1 | đồng bộ trước | cam kết trở thành 7 | 
| 61 | Nhận (3,2) | 1 | đọc cam kết | 7 | 

Ví dụ này nêu bật quy tắc chính: tại thời điểm t = 60, quá trình đồng bộ hóa diễn ra trước truy vấn, do đó các bản cập nhật đang chờ xử lý sẽ được xóa trước tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q + u) | mỗi truy vấn là O(1), đồng bộ hóa chỉ xử lý các ô được cập nhật | 
| Không gian | O(n + u) | mảng đã cam kết cộng với tối đa một mục đang chờ xử lý cho mỗi lần cập nhật (sid, pos) | 

Giải pháp này phù hợp thoải mái trong giới hạn vì số lượng cập nhật thực tế u nhiều nhất là q và mỗi cập nhật được xử lý với số lần không đổi: một lần khi được chèn và một lần khi bị xóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib
    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like small case
assert run("""2 2 10
1 1
3
4 Get 1 1
5 Set 1 1 5
6 Get 1 1
""") == "1\n5"

# sync boundary behavior
assert run("""1 3 5
1 2 3
3
5 Get 1 1
5 Get 1 1
6 Get 1 1
""") == "1\n1\n1"

# multiple overwrites in same cycle
assert run("""2 3 100
0 0 0
4
1 Set 1 1 2
2 Set 1 1 3
3 Get 1 1
4 Get 1 1
""") == "3\n3"

# large cycle gap
assert run("""1 2 10
5 5
2
1 Get 1 1
11 Get 1 1
""") == "5\n5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bộ nhỏ/chuỗi nhận được | 1, 5 | tính chính xác ghi đè cơ bản | 
| ranh giới tại thời điểm đồng bộ | lặp đi lặp lại 1 giây | quy tắc đồng bộ hóa trước truy vấn | 
| ghi đè lên cùng một khóa | 3, 3 | lần ghi cuối cùng trong chu kỳ | 
| nhảy chu kỳ | 5, 5 | lười biếng đồng bộ hóa chính xác | 

## Vỏ cạnh 

Trường hợp biên quan trọng là khi một truy vấn đến chính xác ranh giới đồng bộ hóa. Đối với đầu vào có s = 5 và t = 5, việc đồng bộ hóa phải diễn ra trước khi đọc các bản cập nhật đang chờ xử lý. 

đầu vào: 

k = 1, n = 1, s = 5 

một = [10] 

5 Bộ 1 1 99 

5 Nhận 1 1 

Tại thời điểm thứ 5, Set được áp dụng sau khi đồng bộ hóa, nhưng Get cũng xảy ra sau khi đồng bộ hóa. Bản cập nhật đang chờ xử lý tại thời điểm thứ 5 thuộc về chu kỳ mới nên không hiển thị trong Get. 

Theo dõi điều này trong thuật toán, khi t = 5, trước tiên chúng tôi phát hiện thay đổi chu kỳ và xóa đang chờ xử lý (trống), sau đó xử lý Lưu trữ tập hợp (5,99). Nhận tiếp theo sẽ thấy đang chờ xử lý và trả về 99. 

Một trường hợp khác là nhiều bản cập nhật cho các máy chủ khác nhau cho cùng một vị trí trong một chu kỳ. Chỉ có dấu thời gian mới nhất mới quan trọng nên các lần ghi trước đó sẽ bị loại bỏ âm thầm trong quá trình đồng bộ hóa.
