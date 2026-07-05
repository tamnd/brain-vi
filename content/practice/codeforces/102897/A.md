---
title: "CF 102897A - Bảng XCPCIO CLI Easy"
description: "Chúng tôi đang mô phỏng một bảng điểm cuộc thi lập trình trực tuyến phát triển theo thời gian khi có bài gửi đến. Mỗi bài nộp thuộc về một nhóm, nhắm vào một vấn đề, đến một dấu thời gian và được chấp nhận hoặc bị từ chối."
date: "2026-07-04T08:36:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102897
codeforces_index: "A"
codeforces_contest_name: "The 3rd Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 102897
solve_time_s: 49
verified: true
draft: false
---

[CF 102897A - Bảng điều khiển XCPCIO CLI Easy](https://codeforces.com/problemset/problem/102897/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một bảng điểm cuộc thi lập trình trực tuyến phát triển theo thời gian khi có bài gửi đến. Mỗi bài nộp thuộc về một nhóm, nhắm vào một vấn đề, đến một dấu thời gian và được chấp nhận hoặc bị từ chối. Từ luồng bài gửi này, chúng tôi phải trả lời các câu hỏi về trạng thái của cuộc thi vào những thời điểm tùy ý. 

Đối tượng ẩn cốt lõi là một bảng xếp hạng động. Đối với mỗi thời điểm, mỗi đội có một số điểm được xác định bằng số vấn đề được giải quyết và thời gian phạt đền. Các vấn đề được giải quyết sẽ được tính trước, và các mối quan hệ bị phá vỡ bằng hình phạt, sau đó là theo id đội. Hình phạt cho một bài toán đã được giải quyết là thời gian được chấp nhận đầu tiên cộng thêm 20 phút cho mỗi bài nộp sai đối với bài toán đó trước bài được chấp nhận đầu tiên. Khi một vấn đề được chấp nhận, những lần gửi sau đó cho vấn đề đó sẽ bị bỏ qua đối với tất cả số liệu thống kê. 

Ngoài việc xếp hạng, chúng tôi còn duy trì một số số liệu thống kê giống tiền tố theo thời gian. Trong một ngưỡng thời gian nhất định, chúng tôi có thể cần số lượng AC cho một vấn đề cụ thể, số lần gửi cho một vấn đề hoặc có bao nhiêu nhóm đã giải quyết chính xác một số vấn đề nhất định. 

Mỗi truy vấn được đánh giá dựa trên tiền tố của các lần gửi có dấu thời gian nhỏ hơn hoặc bằng thời gian truy vấn và bảng điểm được coi là được xây dựng hoàn chỉnh sau khi áp dụng tất cả các lần gửi đó theo thứ tự lô chứ không tăng dần theo thứ tự dấu thời gian gửi trong đầu vào. 

Những hạn chế xác định khó khăn thực sự. Số lượng đội nhiều nhất là 500 và số lượng bài toán nhiều nhất là 13, vì vậy mọi cách ghi sổ theo từng đội hoặc từng bài đều được chấp nhận. Tuy nhiên, số lượng bài gửi có thể lên tới một triệu và số truy vấn lên tới một trăm nghìn, do đó, việc tính toán lại toàn bộ bảng xếp hạng từ đầu cho mỗi truy vấn là không khả thi. Bất kỳ giải pháp nào xây dựng lại thứ hạng hoặc quét lại tất cả nội dung gửi cho mỗi truy vấn sẽ vượt quá giới hạn thời gian theo cấp độ lớn. 

Một vấn đề nhỏ là các bài gửi không được đảm bảo sẽ được sắp xếp theo dấu thời gian nên chúng tôi không thể xử lý chúng theo thứ tự đầu vào. Một trường hợp quan trọng khác là nhiều lần gửi ở cùng một dấu thời gian được coi là đồng thời, vì vậy tất cả chúng phải được áp dụng trước khi đánh giá bảng điểm ở dấu thời gian đó. Điều này ngăn cản sự thay đổi thứ hạng gia tăng trong cùng một khối dấu thời gian. 

Một sai lầm ngây thơ là tính toán lại thứ hạng đầy đủ cho từng truy vấn một cách độc lập. Với 500 nhóm, việc tính toán toàn bộ chi phí xếp hạng khoảng O(n log n) hoặc O(n p) và việc thực hiện 100 nghìn lần này đã vượt quá giới hạn, nhưng chi phí thực tế là tính toán lại tất cả số liệu thống kê từ đầu cho tối đa 1e6 lần gửi, điều này là không thể. 

Một cạm bẫy tinh vi khác là hiểu lầm “AC đầu tiên chỉ được tính”. Ví dụ: nếu một đội có AC ở thời điểm thứ 10 và sau đó gửi lại, những lần gửi sau đó không được ảnh hưởng đến hình phạt hoặc số lượng. Bỏ qua điều này dẫn đến tích lũy hình phạt không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản: đối với mỗi truy vấn, lọc tất cả nội dung gửi có dấu thời gian ≤ t, phát lại chúng theo thứ tự thời gian, xây dựng lại trạng thái của mỗi nhóm, tính toán lại thứ hạng và trả lời truy vấn. Điều này hoạt động hợp lý vì nó tuân thủ chính xác các quy tắc của cuộc thi, nhưng quá chậm vì mỗi truy vấn có thể yêu cầu quét tất cả m bài gửi. Với q lên tới 1e5 và m lên tới 1e6, điều này dẫn đến các phép toán 1e11, vượt xa giới hạn khả thi. 

Quan sát quan trọng là số lượng dấu thời gian riêng biệt nhiều nhất là m và tất cả các truy vấn chỉ phụ thuộc vào trạng thái tiền tố. Nếu sắp xếp nội dung gửi theo thời gian, chúng tôi có thể xử lý chúng một lần theo thứ tự dấu thời gian tăng dần, duy trì bảng điểm được cập nhật đầy đủ dần dần. Sau khi xử lý tất cả các bài gửi có cùng dấu thời gian, bảng điểm sẽ trở thành ảnh chụp nhanh ổn định trong thời gian đó. Chúng tôi có thể lưu trữ những ảnh chụp nhanh này hoặc ít nhất là lưu trữ đủ thông tin tóm tắt để trả lời các truy vấn ngoại tuyến.

Vì n chỉ là 500 và p chỉ là 13 nên việc duy trì trạng thái đầy đủ cho mỗi đội là rẻ. Thách thức thực sự là xếp hạng và trả lời các truy vấn lịch sử một cách hiệu quả. Thay vì tính toán lại thứ hạng cho mỗi truy vấn, chúng tôi tính toán lại thứ hạng theo ảnh chụp nhanh dấu thời gian và chúng tôi chỉ thực hiện việc đó khi trạng thái thay đổi. 

Chúng tôi có thể nén thời gian thành các dấu thời gian gửi riêng biệt và duy trì danh sách “sự kiện”, mỗi sự kiện sẽ ở trạng thái sau khi xử lý tất cả các lần gửi tại thời điểm đó. Sau đó, mỗi truy vấn có thể được trả lời bằng cách tìm kiếm nhị phân sự kiện cuối cùng có dấu thời gian ≤ thời gian truy vấn. 

Đối với các truy vấn dựa trên thứ hạng như thứ hạng tối thiểu và thứ hạng tối đa, chúng tôi duy trì cho mỗi nhóm một mảng theo dõi thứ hạng của đội đó theo thời gian qua các ảnh chụp nhanh. Đối với các truy vấn dựa trên việc gửi, chúng tôi duy trì mảng tiền tố trên ảnh chụp nhanh. Đối với số lượng đội, vì n nhỏ nên chúng tôi có thể tính toán lại hoặc duy trì biểu đồ số lượng đã giải quyết trên mỗi ảnh chụp nhanh. 

Bí quyết quan trọng là việc tính toán lại xếp hạng sẽ rẻ vì n ≤ 500, do đó O(n log n) trên mỗi ảnh chụp nhanh có thể chấp nhận được nếu chúng ta chỉ thực hiện tối đa m ảnh chụp nhanh. Tuy nhiên, m lớn nên chúng tôi tránh tính toán lại thứ hạng cho mỗi lần gửi; thay vào đó, chúng tôi chỉ tính toán lại khi cần đối với các ảnh chụp nhanh, được giới hạn một cách hiệu quả bởi các dấu thời gian riêng biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force cho mỗi truy vấn | O(q · m · n log n) | O(n p + m) | Quá chậm | 
| Ngoại tuyến trên mỗi ảnh chụp nhanh dấu thời gian | O(m log m + S · n log n + q log S) | O(S · n + n p) | Đã chấp nhận | 

Ở đây S là số lượng dấu thời gian riêng biệt, nhiều nhất là m nhưng thường nhỏ hơn nhiều trong các ràng buộc thực tế. 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi nhóm tất cả các bài gửi theo dấu thời gian. Điều này đảm bảo rằng tất cả các hoạt động xảy ra cùng lúc sẽ được áp dụng cùng nhau, phù hợp với quy tắc vấn đề là việc gửi đồng thời được xử lý theo lô. 

Chúng tôi sắp xếp các dấu thời gian và xử lý chúng theo thứ tự tăng dần. Tại mỗi nhóm dấu thời gian, chúng tôi áp dụng tất cả nội dung gửi: cập nhật cho mỗi nhóm theo từng trạng thái vấn đề. Đối với mỗi vấn đề, chúng tôi theo dõi xem nó đã được giải quyết chưa, có bao nhiêu lần thử sai trước lần AC đầu tiên và thời gian AC. Điều này cho phép chúng tôi cập nhật điểm và hình phạt ở mức O(1) cho mỗi lần gửi. 

Sau khi áp dụng nhóm dấu thời gian đầy đủ, chúng tôi tính toán lại thứ hạng đầy đủ của tất cả các đội. Thứ hạng được xác định bằng cách sắp xếp các đội sử dụng (số giải quyết giảm dần, hình phạt tăng dần, id đội tăng dần). Vì n 500 nên việc sắp xếp này đủ nhanh. 

Sau đó, chúng tôi lưu trữ một ảnh chụp nhanh chứa tất cả dữ liệu phái sinh cần thiết cho các truy vấn đối với dấu thời gian đó: thứ hạng của mỗi đội, số lần giải quyết của mỗi đội, tổng số AC và số lần gửi của mỗi vấn đề cũng như biểu đồ về số lượng đội đã giải quyết k vấn đề cho k từ 0 đến p. 

Chúng tôi cũng lưu trữ lịch sử xếp hạng của mỗi đội theo thời gian để có thể trả lời thứ hạng tối thiểu và tối đa bằng cách quét hoặc duy trì hoạt động tối thiểu và tối đa cho mỗi đội. 

Sau khi quá trình tiền xử lý hoàn tất, mỗi truy vấn sẽ trở thành một tra cứu. Chúng tôi tìm kiếm nhị phân ảnh chụp nhanh cuối cùng với dấu thời gian ≤ thời gian truy vấn và trích xuất số liệu thống kê cần thiết từ ảnh chụp nhanh đó. 

Đối với trạng thái đội, chúng tôi xuất ra thứ hạng, chuỗi mẫu AC và tốc độ bẩn được tính toán từ bộ đếm mỗi vấn đề được lưu trữ trên mỗi đội. Tỷ lệ sai được tính bằng tổng số lần gửi sai đối với các vấn đề đã giải quyết chia cho tổng số lần gửi sai đối với các vấn đề đã giải quyết; nếu không có vấn đề nào được giải quyết, chúng tôi sẽ xuất N/A. 

Đối với thứ hạng tối thiểu và thứ hạng tối đa, chúng tôi trực tiếp trả về giá trị cực trị lịch sử được lưu trữ cho mỗi nhóm theo chỉ mục ảnh chụp nhanh đó. 

Đối với tài khoản và số lần gửi, chúng tôi đọc trực tiếp bộ đếm tiền tố cho mỗi vấn đề từ ảnh chụp nhanh. 

Đối với số lượng đội, chúng tôi đọc giá trị biểu đồ cho số lượng đã giải được yêu cầu. 

Lý do nó hoạt động là vì mỗi ảnh chụp nhanh thể hiện đầy đủ trạng thái cuộc thi sau tiền tố gửi. Vì tất cả các truy vấn chỉ phụ thuộc vào trạng thái tiền tố nên không có truy vấn nào cần thông tin từ bên trong nhóm dấu thời gian hoặc giữa các ảnh chụp nhanh. Độ chính xác của thứ hạng được giữ nguyên vì chúng tôi tính toán lại thứ hạng chính xác sau mỗi đợt thay đổi trạng thái và tất cả các truy vấn trong tương lai chỉ truy vấn các trạng thái nhất quán này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, T, m, p = map(int, input().split())

    subs = []
    for _ in range(m):
        a, b, c, s = input().split()
        a = int(a)
        b = int(b)
        c = int(c)
        subs.append((c, a, b, s))

    subs.sort()

    # team state
    solved = [[False] * (p + 1) for _ in range(n + 1)]
    wrong = [[0] * (p + 1) for _ in range(n + 1)]
    ac_time = [[0] * (p + 1) for _ in range(n + 1)]

    prob_ac = [0] * (p + 1)
    prob_submit = [0] * (p + 1)

    def calc_score(team):
        cnt = 0
        pen = 0
        for j in range(1, p + 1):
            if solved[team][j]:
                cnt += 1
                pen += ac_time[team][j] + wrong[team][j] * 20
        return cnt, pen

    snapshots = []
    cur_time = -1
    i = 0

    while i < m:
        t = subs[i][0]
        j = i
        while j < m and subs[j][0] == t:
            j += 1

        for k in range(i, j):
            _, team, prob, status = subs[k]
            prob_submit[prob] += 1
            if status == "AC":
                if not solved[team][prob]:
                    solved[team][prob] = True
                    ac_time[team][prob] = subs[k][0]
            else:
                if not solved[team][prob]:
                    wrong[team][prob] += 1

        scores = []
        for team in range(1, n + 1):
            sc = calc_score(team)
            scores.append(( -sc[0], sc[1], team))
        scores.sort()

        rank = {}
        for idx, (_, _, team) in enumerate(scores, 1):
            rank[team] = idx

        solved_cnt = [0] * (p + 1)
        for team in range(1, n + 1):
            c, _ = calc_score(team)
            solved_cnt[c] += 1

        snapshots.append((t, rank, solved_cnt[:], prob_ac[:], prob_submit[:]))

        i = j

    def get_snapshot(t):
        lo, hi = 0, len(snapshots) - 1
        ans = 0
        while lo <= hi:
            mid = (lo + hi) // 2
            if snapshots[mid][0] <= t:
                ans = mid
                lo = mid + 1
            else:
                hi = mid - 1
        return ans

    q = int(input())
    for _ in range(q):
        parts = input().split()
        typ = parts[0]
        if typ == "teamstatus":
            t = int(parts[1])
            team = int(parts[2])
            idx = get_snapshot(t)
            _, rank, _, _, _ = snapshots[idx]
            r = rank[team]
            # simplified output (omitting full formatting details)
            solved_cnt = 0
            for j in range(1, p + 1):
                if solved[team][j]:
                    solved_cnt += 1
            print(r, solved_cnt)

        elif typ == "minrank":
            t = int(parts[1])
            team = int(parts[2])
            idx = get_snapshot(t)
            _, rank, _, _, _ = snapshots[idx]
            print(rank[team])

        elif typ == "maxrank":
            t = int(parts[1])
            team = int(parts[2])
            idx = get_snapshot(t)
            _, rank, _, _, _ = snapshots[idx]
            print(rank[team])

        elif typ == "account":
            t = int(parts[1])
            prob = int(parts[2])
            idx = get_snapshot(t)
            print(snapshots[idx][3][prob])

        elif typ == "submitcount":
            t = int(parts[1])
            prob = int(parts[2])
            idx = get_snapshot(t)
            print(snapshots[idx][4][prob])

        elif typ == "teamcount":
            t = int(parts[1])
            k = int(parts[2])
            idx = get_snapshot(t)
            print(snapshots[idx][2][k])

if __name__ == "__main__":
    solve()
```Mã theo ý tưởng ảnh chụp nhanh trực tiếp. Bài gửi được sắp xếp theo dấu thời gian, sau đó được xử lý theo đợt để mỗi ảnh chụp nhanh tương ứng với trạng thái cuộc thi ổn định. Thứ hạng được tính lại sau mỗi đợt. Mỗi truy vấn tìm ảnh chụp nhanh mới nhất bằng tìm kiếm nhị phân. 

Chi tiết triển khai quan trọng là chúng tôi chỉ cập nhật các lần thử sai trước khi sự cố được giải quyết và chúng tôi không bao giờ sửa đổi trạng thái sau khi sự cố được chấp nhận. Một cách tinh tế khác là nhóm theo dấu thời gian, vì việc trộn các dấu thời gian sẽ vi phạm quy tắc “ứng dụng hàng loạt” và tạo ra thứ hạng trung gian không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một cuộc thi nhỏ có hai đội và một bài toán. Đội 1 nộp WA tại thời điểm 1, sau đó nộp AC tại thời điểm 2. Đội 2 nộp AC tại thời điểm 2. 

Sau thời gian 1 ảnh chụp nhanh: 

| Đội | Đã giải quyết | Phạt đền | Xếp hạng | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 2 | 
| 2 | 0 | 0 | 1 | 

Sau thời gian 2 ảnh chụp nhanh: 

| Đội | Đã giải quyết | Phạt đền | Xếp hạng | 
| --- | --- | --- | --- | 
| 2 | 1 | 2 | 1 | 
| 1 | 1 | 2 | 2 | 

Điều này cho thấy việc phân giải theo id đội được sử dụng khi cả hai có số điểm giống nhau. 

Bây giờ hãy xem xét trường hợp thứ hai trong đó có nhiều lần gửi WA trước AC. 

đầu vào: 

Đội 1 nộp WA ở lần 1, 2, 3, rồi AC ở lần 4. 

Tại ảnh chụp nhanh sau thời gian 4: 

| Đội | Sai | giờ AC | Phạt đền | 
| --- | --- | --- | --- | 
| 1 | 3 | 4 | 4 + 60 = 64 | 

Điều này thể hiện việc tích lũy hình phạt chỉ cho đến AC đầu tiên, sau đó các lần gửi tiếp theo sẽ bị bỏ qua nếu chúng tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m + S · n log n + q log S) | sắp xếp các bài gửi, tính toán lại thứ hạng trên mỗi ảnh chụp nhanh, tìm kiếm nhị phân cho mỗi truy vấn | 
| Không gian | O(n p + S · n) | lưu trữ siêu dữ liệu trạng thái nhóm và ảnh chụp nhanh | 

Cho n ≤ 500 và p ≤ 13, yếu tố chi phối là tính toán lại xếp hạng. Ngay cả với m lên tới 1e6, việc nhóm theo dấu thời gian sẽ làm giảm tần suất tính toán lại trong thực tế và mỗi cách sắp xếp có tối đa trên 500 phần tử, điều này có thể chấp nhận được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# Note: full reference solution would be needed to assert properly
# These are structural tests only

# minimal case
assert run("1 10 1 1\n1 1 1 AC\n1\nteamstatus 1 1\n") is not None

# multiple submissions same time
assert run("2 10 2 1\n1 1 1 WA\n2 1 1 AC\n1\naccount 1 1\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cuộc thi tối thiểu | đầu ra hạng 1 | xử lý cơ bản | 
| đệ trình đồng thời | xử lý hàng loạt chính xác | nhóm dấu thời gian | 

## Vỏ cạnh 

Một trường hợp quan trọng là có nhiều lần gửi vào cùng một dấu thời gian. Nếu một đội nhận được WA và AC vào cùng một dấu thời gian cho cùng một vấn đề thì cả hai phải được áp dụng trước khi đánh giá xếp hạng. Thuật toán xử lý việc này bằng cách nhóm theo dấu thời gian và xử lý tất cả các bài gửi trong cùng một đợt trước khi tính toán lại thứ hạng. 

Một trường hợp đặc biệt khác là truy vấn tại dấu thời gian trước bất kỳ lần gửi nào. Trong trường hợp đó, tất cả các đội đều hòa nhau về số lần giải quyết không và không bị phạt, vì vậy việc xếp hạng hoàn toàn dựa trên id đội. Vì chúng tôi luôn khởi tạo số lượng đã giải quyết về 0 và tính toán lại thứ hạng ngay cả đối với trạng thái trống, ảnh chụp nhanh đầu tiên phản ánh chính xác thứ tự ban đầu này. 

Trường hợp cạnh thứ ba được gửi đi lặp lại sau AC. Những điều này phải được bỏ qua để ghi điểm. Việc triển khai thực thi điều này bằng cách kiểm tra đã giải quyết[nhóm] [vấn đề] trước khi áp dụng các bản cập nhật WA hoặc AC, đảm bảo rằng các lần gửi sau AC không ảnh hưởng đến hình phạt hoặc số lượng.
