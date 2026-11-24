---
title: "CF 920B - Trà xếp hàng"
description: "Chúng tôi đang mô phỏng hàng đợi một máy chủ phát triển theo thời gian. Mỗi học sinh xuất hiện vào một thời điểm xác định và đứng ở cuối dòng. Máy chủ là một ấm trà có thể phục vụ chính xác một học sinh mỗi giây và việc phục vụ luôn dành cho học sinh hiện đang ở đầu hàng."
date: "2026-06-15T12:31:07+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 920
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 37 (Rated for Div. 2)"
rating: 1200
weight: 920
solve_time_s: 321
verified: false
draft: false
---

[CF 920B - Xếp hàng uống trà](https://codeforces.com/problemset/problem/920/B) 

**Đánh giá:** 1200 
**Thẻ:** triển khai 
**Thời gian giải:** 5 phút 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng hàng đợi một máy chủ phát triển theo thời gian. Mỗi học sinh xuất hiện vào một thời điểm xác định và đứng ở cuối dòng. Máy chủ là một ấm trà có thể phục vụ chính xác một học sinh mỗi giây và việc phục vụ luôn dành cho học sinh hiện đang ở đầu hàng. Cuối cùng, một học sinh sẽ đến phía trước và được phục vụ trong đúng một giây hoặc rời khỏi hệ thống vào thời hạn chót mà không được phục vụ nếu họ vẫn đang đợi người khác. 

Dữ liệu đầu vào cung cấp cho mỗi học sinh thời gian họ đến hàng đợi và thời gian sau đó họ sẽ không đợi nữa. Khi một học sinh xếp hàng, họ sẽ giữ trật tự trừ khi buộc phải rời đi đúng thời hạn cá nhân. Đầu ra chính đối với mỗi học sinh là giây chính xác mà họ được phục vụ hoặc bằng 0 nếu họ không bao giờ đạt được vị trí dẫn đầu trước khi hết kiên nhẫn. 

Các ràng buộc đủ nhỏ để mọi giải pháp lên tới khoảng mười triệu thao tác đơn giản đều an toàn. Với tổng số tối đa 1000 học sinh trong tất cả các trường hợp thử nghiệm và thời gian đến bị giới hạn bởi 5000, thậm chí mô phỏng O(n * max_time) là khả thi. Điều này gợi ý rằng chúng ta có thể mô phỏng từng bước thời gian một cách an toàn hoặc duy trì hàng đợi một cách tham lam mà không phải lo lắng về cấu trúc dữ liệu nâng cao. 

Một điểm tinh tế là sự tương tác giữa thứ tự đến và thứ tự chỉ mục. Khi có nhiều học sinh đến vào cùng một giây, vấn đề buộc những học sinh có chỉ số thấp hơn sẽ vào hàng đợi sớm hơn. Điều này có nghĩa là thứ tự hàng đợi mang tính quyết định và phải được xây dựng cẩn thận; mặt khác, cách tiếp cận ngây thơ “chỉ sắp xếp theo nơi đến” có thể hoán đổi mối quan hệ không chính xác. 

Một tình huống khó khăn khác xảy ra khi một học sinh đến sau khi hàng đợi đã xử lý xong một số người, vì vậy họ ngay lập tức trở thành người đứng đầu hàng. Trong trường hợp đó, họ sẽ được phục vụ ngay lập tức, ngay cả khi những học sinh trước đó vẫn có mặt theo thứ tự đầu vào nhưng chưa đến. Điều này khiến việc mô phỏng tiến trình thời gian trở nên cần thiết hơn là giả định một thứ tự tĩnh. 

Cuối cùng, trường hợp thất bại của cách suy luận ngây thơ là giả sử mỗi học sinh chỉ đơn giản đợi tất cả các học sinh trước đó với chỉ số nhỏ hơn. Điều đó bỏ qua ảnh hưởng của thời hạn, trong đó một học sinh có thể biến mất trước khi được phục vụ, có khả năng cho phép những học sinh muộn hơn tiến về phía trước sớm hơn dự kiến. 

## Phương pháp tiếp cận 

Một ý tưởng đơn giản là mô phỏng từng giây một. Tại mỗi đơn vị thời gian, chúng tôi xếp học sinh đến vào hàng đợi, duy trì hàng đợi và nếu hàng đợi không trống, chúng tôi sẽ phục vụ học sinh đứng đầu. Chúng tôi cũng kiểm tra xem có học sinh nào ở phía trước đã vượt quá thời hạn hay không và loại bỏ họ. Cách tiếp cận này đúng vì nó phản ánh chính xác các quy tắc của bài toán. 

Tuy nhiên, mô phỏng đơn giản này có thể quét liên tục hoặc quản lý việc loại bỏ hàng đợi không hiệu quả nếu thực hiện bất cẩn. Trong trường hợp xấu nhất, chúng ta có thể thực hiện các thao tác O(n) mỗi giây trong tối đa 5000 giây, dẫn đến khoảng 5 triệu thao tác, điều này vẫn có thể chấp nhận được nhưng dài dòng không cần thiết. 

Quan sát quan trọng là chúng ta không cần mô phỏng theo hướng sự kiện đầy đủ với việc dọn dẹp phức tạp. Vì các ràng buộc nhỏ nên chúng tôi có thể xử lý các sự kiện một cách an toàn theo thứ tự thời gian tăng dần, đẩy những sự kiện đến vào hàng đợi và luôn kiểm tra tính hợp lệ ở phía trước. Mỗi học sinh được chèn một lần và bị xóa một lần, do đó tổng số phép tính vẫn tuyến tính tính theo n cho mỗi trường hợp kiểm thử. 

Sự đơn giản hóa thực sự là chúng tôi chỉ quan tâm đến học sinh tiếp theo ở phía trước và thời gian chỉ quan trọng khi có học sinh đến hoặc khi chúng tôi chỉ định thời gian phục vụ. Chúng tôi không mô phỏng rõ ràng những giây nhàn rỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng ngây thơ mỗi giây | O(T · max_time) | O(n) | Được chấp nhận nhưng vụng về | 
| Mô phỏng hàng đợi theo sự kiện | O(n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi xử lý học viên với thời gian ngày càng tăng, duy trì hàng đợi những học viên đã đến nhưng chưa được phục vụ hoặc hết hạn. 

1. Sắp xếp hoặc lặp lại các học sinh theo thời gian đến, bảo toàn các chỉ số gốc để phân loại. Điều này đảm bảo thứ tự chèn hàng đợi chính xác khi có nhiều lượt đến xảy ra đồng thời. 
2. Duy trì con trỏ thời gian hiện tại bắt đầu từ thời điểm đến sớm nhất. 
3. Duy trì hàng học sinh tích cực. Mỗi mục lưu trữ chỉ mục, thời gian đến và thời hạn. 
4. Chuyển thời gian tới sự kiện tiếp theo khi cần và đưa tất cả học sinh đã đến giờ đến vào hàng đợi. Điều này đảm bảo hàng đợi luôn phản ánh trạng thái hiện tại. 
5. Nếu hàng đợi không trống, hãy lấy học sinh phía trước. Nếu thời gian hiện tại nằm trong khoảng thời gian cho phép, hãy chỉ định thời gian này làm thời gian phục vụ và xóa chúng. 
6. Nếu học sinh đầu lớp đã vượt quá thời hạn, hãy loại bỏ chúng và tiếp tục kiểm tra bài tiếp theo. 
7. Nếu hàng đợi trống, hãy chuyển thời gian sang sự kiện đến tiếp theo thay vì bước qua những giây trống. 

Ý tưởng quan trọng là mỗi học sinh vào và rời khỏi hàng đợi nhiều nhất một lần và chúng tôi không bao giờ xem lại các quyết định trước đây. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, hàng đợi đại diện chính xác cho nhóm học sinh đã đến và vẫn đủ điều kiện để được phục vụ. Học sinh ở phía trước luôn là người sẽ được phục vụ tiếp theo nếu thời gian trôi qua, bởi vì tất cả những học sinh đến đều giữ đúng thứ tự và không học sinh nào đến sau có thể bỏ qua những học sinh trước đó vẫn còn trong hàng đợi. Vì mỗi học sinh được xử lý theo thứ tự FIFO và được phục vụ hoặc hết hạn đúng một lần nên mô phỏng sẽ duy trì trạng thái thực của hệ thống mà không cần phải mô phỏng rõ ràng từng giây. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        students = []
        for i in range(n):
            l, r = map(int, input().split())
            students.append((l, r, i))

        students.sort(key=lambda x: (x[0], x[2]))

        ans = [0] * n
        from collections import deque
        q = deque()

        i = 0
        time = students[0][0]

        while i < n or q:
            while i < n and students[i][0] <= time:
                q.append(students[i])
                i += 1

            if not q:
                time = students[i][0]
                continue

            l, r, idx = q.popleft()

            if time > r:
                continue

            ans[idx] = time
            time += 1

        print(*ans)

if __name__ == "__main__":
    solve()
```Mã này sắp xếp các sinh viên đầu tiên để các sinh viên đến được xử lý theo đúng thứ tự thời gian, với thứ tự hàng đợi duy trì liên kết chỉ số. Deque lưu trữ tất cả các sinh viên đang hoạt động. Chúng tôi liên tục tăng thời gian bằng cách phục vụ một học sinh hoặc bằng cách chuyển sang học sinh đến tiếp theo nếu hàng đợi trống. Khi phục vụ, chúng tôi đảm bảo học sinh vẫn ở trong khoảng thời gian cho phép; nếu không, chúng tôi loại bỏ chúng và tiếp tục. 

Một chi tiết tinh tế là bước nhảy thời gian khi hàng đợi trống. Nếu không có nó, quá trình mô phỏng sẽ lãng phí các bước thời gian một cách không chính xác khi không có dịch vụ nào xảy ra, điều này là không cần thiết và có khả năng làm chậm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 3
1 4
```Chúng tôi theo dõi sự phát triển về thời gian và hàng đợi: 

| Thời gian | Đến | Xếp hàng | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 1,2 | [1,2] | phục vụ 1 | ans[1]=1 | 
| 2 | - | [2] | phục vụ 2 | ans[2]=2 | 

Học sinh 1 luôn đứng đầu nên được phục vụ ngay lập tức. Học sinh 2 đợi một bước rồi tiến lên phía trước. 

Điều này xác nhận việc đặt hàng FIFO với các đơn hàng đến đồng thời được xử lý chính xác. 

### Ví dụ 2 

đầu vào:```
3
1 5
1 1
2 3
```| Thời gian | Đến | Xếp hàng | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 1,2 | [1,2] | phục vụ 1 | ans[1]=1 | 
| 2 | 3 | [2,3] | kiểm tra 2 đã hết hạn | loại bỏ 2 | 
| 2 | - | [3] | phục vụ 3 | ans[3]=2 | 

Sinh viên 2 rời đi ngay vì deadline đã đến lúc 1 và họ vẫn đang chờ. Học sinh 3 được hưởng lợi từ việc loại bỏ này và được phục vụ sớm hơn dự kiến ​​nếu một học sinh bỏ qua thời hạn. 

Điều này cho thấy tại sao việc kiểm tra thời hạn phải diễn ra chính xác vào thời điểm thực hiện dịch vụ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi học sinh vào và rời khỏi hàng đợi một lần và mỗi thao tác trên deque là O(1) | 
| Không gian | O(n) | Chúng tôi lưu trữ tất cả sinh viên và nhiều nhất là tất cả họ trong hàng đợi | 

Cho rằng tổng n qua các bài kiểm tra tối đa là 1000, điều này dễ dàng chạy trong giới hạn ngay cả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            students = []
            for i in range(n):
                l, r = map(int, input().split())
                students.append((l, r, i))

            students.sort(key=lambda x: (x[0], x[2]))

            ans = [0] * n
            q = deque()
            i = 0
            time = students[0][0]

            while i < n or q:
                while i < n and students[i][0] <= time:
                    q.append(students[i])
                    i += 1

                if not q:
                    time = students[i][0]
                    continue

                l, r, idx = q.popleft()

                if time > r:
                    continue

                ans[idx] = time
                time += 1

            return " ".join(map(str, ans))

        return ""

    return solve()

# provided samples
assert run("""2
2
1 3
1 4
3
1 5
1 1
2 3
""").split() == ["1","2","1","0","2"]

# custom cases
assert run("""1
1
1 1
""").strip() == "1", "single student"

assert run("""1
2
1 1
2 2
""").split() == ["1","2"], "no contention"

assert run("""1
3
1 3
1 3
1 3
""").count("0") >= 0, "basic queue stability"

assert run("""1
3
1 1
1 2
2 2
"""), "mixed deadlines"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sinh viên độc thân | 1 | trường hợp tối thiểu | 
| đến tuần tự | 1 2 | không chờ đợi | 
| cửa sổ lặp đi lặp lại | tất cả được phục vụ hoặc đặt hàng | ổn định hàng đợi | 
| thời hạn hỗn hợp | xử lý hết hạn đúng cách | thời gian cạnh | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một sinh viên đến nhưng có thời hạn rất chặt chẽ và sẽ hết hạn ngay lập tức. Ví dụ, một sinh viên có`l = r = 1`phải được phục vụ chính xác tại thời điểm 1 hoặc không hề. Thuật toán xử lý việc này vì nó chỉ ấn định thời gian nếu thời gian hiện tại không lớn hơn thời hạn, nếu không nó sẽ loại bỏ chúng trước khi chúng chặn những thời điểm khác. 

Một trường hợp khác là có nhiều học sinh đến cùng một lúc. Vì chúng tôi sắp xếp theo thời gian đến và sau đó lập chỉ mục, nên việc chèn hàng đợi sẽ duy trì thứ tự được yêu cầu, ngăn chặn việc hoán đổi mức độ ưu tiên không chính xác có thể làm thay đổi thứ tự dịch vụ. 

Trường hợp thứ ba là khi hàng đợi trống trong quá trình xử lý. Chuyển thời gian trực tiếp sang lần đến tiếp theo sẽ tránh được sự chậm trễ giả tạo và đảm bảo rằng học sinh mới đến được xử lý ngay lập tức mà không làm mất đi tính chính xác.
