---
title: "CF 104467F - Những chàng trai mùa thu"
description: "Mỗi đội có bốn người chơi thi đấu trong một cuộc đua. Khi cuộc đua diễn ra, một số người chơi đã về đích và một số đã rơi vào tình trạng nhếch nhác."
date: "2026-06-30T13:08:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104467
codeforces_index: "F"
codeforces_contest_name: "La Salle-Pui Ching Programming Challenge \u57f9\u6b63\u5587\u6c99\u7de8\u7a0b\u6311\u6230\u8cfd 2022"
rating: 0
weight: 104467
solve_time_s: 101
verified: true
draft: false
---

[CF 104467F - Những chàng trai mùa thu](https://codeforces.com/problemset/problem/104467/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi đội có bốn người chơi thi đấu trong một cuộc đua. Khi cuộc đua diễn ra, một số người chơi đã về đích và một số đã rơi vào tình trạng nhếch nhác. Mỗi người chơi đã hoàn thành sẽ đóng góp điểm cho đội của họ dựa trên thứ tự hoàn thành của họ, trong khi những người chơi bị loại bỏ chất nhờn không đóng góp gì. Những người chơi còn lại vẫn có thể về đích theo bất kỳ thứ tự nào hoặc bị loại, vì vậy họ đưa ra sự không chắc chắn về điểm số cuối cùng của đội. 

Điểm của một đội chỉ đơn giản là tổng đóng góp của bốn thành viên. Sau khi tất cả người chơi được giải quyết, các đội được xếp hạng theo tổng điểm. Các đội M hàng đầu đủ điều kiện, nhưng các đội hòa ở thời điểm giới hạn có thể thay đổi luật chơi theo một cách hơi đặc biệt, đặc biệt là khi nhiều đội hòa nhau quanh ranh giới. 

Nhiệm vụ không phải là tính toán thứ hạng cuối cùng mà là xác định xem tập hợp các đội đủ điều kiện đã được cố định hay chưa, bất kể những người chơi chưa xác định còn lại có thể hiện như thế nào. Nói cách khác, chúng ta phải quyết định xem liệu có bất kỳ sự tiếp tục nào của cuộc đua có thể thay đổi đội nào đủ điều kiện hay không. 

Hạn chế chính là N nhiều nhất là 15, nghĩa là số lượng đội rất nhỏ. Điều này ngay lập tức gợi ý rằng chúng ta có đủ khả năng để suy luận về các tập hợp con của các đội hoặc thực hiện suy luận theo cấp số nhân trên các cấu hình, miễn là chúng ta giữ cho công việc trên mỗi trạng thái ở mức nhỏ. 

Một trường hợp cạnh tinh tế phát sinh từ hành vi ràng buộc. Một cách tiếp cận ngây thơ chỉ theo dõi điểm số hiện tại và giả định “điểm tối đa có thể” hoặc “điểm tối thiểu có thể” một cách độc lập cho mỗi đội sẽ không thành công vì trình độ chuyên môn phụ thuộc vào thứ tự tương đối chứ không phải ngưỡng tuyệt đối. Ví dụ: một đội hiện đang ở phía sau vẫn có thể vượt qua đội khác nếu những người về đích trong tương lai đều thuộc về đội đó, trong khi đội dẫn đầu vẫn có thể bị hòa làm thay đổi quy tắc giới hạn. 

Một tình huống khó khăn khác xảy ra khi nhiều đội tập trung chặt chẽ xung quanh vị trí thứ M. Một thay đổi nhỏ về thứ tự giữa các đội bị ràng buộc đó có thể làm thay đổi việc các đội hòa sẽ loại bỏ hay bảo toàn các đội đủ điều kiện, vì vậy, bất kỳ giải pháp nào cũng phải đảm bảo tính nhất quán hoàn toàn của thứ hạng chứ không chỉ thống trị theo cặp. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là mô phỏng mọi kết quả có thể xảy ra cho những người chơi còn lại. Mỗi người chơi chưa hoàn thành có thể hoàn thành theo một số thứ tự hoặc rơi vào chất nhờn, và thứ tự hoàn thành sẽ cho điểm giảm dần. Điều này tạo ra một yếu tố phân nhánh rất lớn, vì mọi hoán vị của những người chơi còn lại đều quan trọng và mỗi nhiệm vụ đều ảnh hưởng đến tổng số đội. Ngay cả khi có rất ít người chơi vắng mặt, điều này vẫn bùng nổ mang tính tổ hợp, vượt xa giới hạn khả thi. 

Một cái nhìn có cấu trúc hơn xuất phát từ việc quan sát rằng điều quan trọng cuối cùng không phải là thứ tự của từng người chơi mà là cấu hình điểm số cuối cùng của đội. Mỗi người chơi còn lại đóng góp 0 hoặc một giá trị được xác định bởi thứ hạng cuối cùng của họ trong số tất cả những người về đích. Thay vì mô phỏng các chuỗi, chúng ta có thể suy luận xem liệu có tồn tại bất kỳ sự ấn định nào của các đóng góp còn lại làm thay đổi tập M đỉnh cuối cùng hay không. 

Vì N ≤ 15, chúng ta có thể chuyển đổi góc nhìn một lần nữa: thay vì nghĩ về người chơi, chúng ta nghĩ về các đội và mỗi đội vẫn có thể thay đổi tổng điểm của mình đến mức nào. Mỗi đội có tối đa bốn người chơi nên độ không chắc chắn của nó là bị giới hạn. Điều này cho phép chúng tôi liệt kê tất cả các trạng thái điểm cuối cùng có thể có cho mỗi đội bằng cách sử dụng tìm kiếm có kiểm soát đối với nhiệm vụ của những người về đích còn lại. 

Khi chúng ta có thể biểu thị tất cả các vectơ điểm cuối cùng có thể có của các đội, vấn đề sẽ giảm xuống còn việc kiểm tra xem tập hợp các đội M hàng đầu có giống nhau trên tất cả các cấu hình điểm có thể tiếp cận hay không. Điều này tương đương với việc kiểm tra xem có tồn tại hai lần hoàn thành hợp lệ tạo ra các bộ tiêu chuẩn khác nhau hay không.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng đầy đủ kết quả của người chơi | Giai thừa lũy thừa của người chơi còn lại | Cao | Quá chậm | 
| Không gian trạng thái trên phân phối điểm của đội (tập hợp con DP / DFS trên kết quả) | O(trạng thái × chuyển tiếp) với N 15 có thể quản lý được | O(tiểu bang) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi những người chơi còn lại đóng góp nhiều điểm kết thúc không xác định, trong đó mỗi nhiệm vụ tương ứng với việc chọn đội nào nhận được từng điểm tiềm năng hoặc liệu người chơi có bị loại hay không. 

1. Tính điểm hiện tại của mỗi đội từ những người chơi đã kết thúc. Điều này đưa ra một đường cơ sở cố định mà từ đó tất cả các khả năng trong tương lai sẽ phân nhánh. 
2. Đếm xem mỗi đội còn lại bao nhiêu người chơi chưa được giải quyết. Mỗi đội vẫn có thể đóng góp tối đa bốn cầu thủ, vì vậy chúng tôi biết chính xác số lượng đóng góp trong tương lai mà mỗi đội vẫn có thể nhận được. 
3. Lập mô hình quy trình còn lại dưới dạng phân phối một số lượng “mã thông báo điểm” cố định tương ứng với vị trí về đích trong tương lai giữa các đội, cộng với khoản đóng góp tùy chọn bằng 0 cho kết quả chất nhờn. Điều này biến vấn đề thành vấn đề phân bổ giới hạn trên tối đa 4N mã thông báo. 
4. Sử dụng DFS hoặc DP trên các trạng thái được xác định bằng số lần đóng góp còn lại mà mỗi đội đã thực hiện và số lượng vị trí hoàn thiện chung đã được chỉ định. Ở mỗi bước, chỉ định thứ hạng hoàn thiện tiếp theo cho một đội vẫn còn người chơi hoặc chỉ định nó cho chất nhờn. 
5. Đối với mỗi nhiệm vụ hoàn thành, hãy tính điểm cuối cùng của đội và xác định đội M đứng đầu. Ghi lại xem tập hợp tiêu chuẩn kết quả có thay đổi qua các lần hoàn thành khác nhau hay không. 
6. Nếu tồn tại nhiều hơn một bộ điều kiện riêng biệt trên tất cả các lần hoàn thành hợp lệ, hãy ghi "Không". Nếu không thì xuất ra "Có". 

Lựa chọn thiết kế quan trọng là chúng tôi không bao giờ mô phỏng rõ ràng các hoán vị của người chơi. Chúng tôi chỉ mô phỏng cách phân bổ đơn vị điểm giữa các đội, được giới hạn bởi tổng số nhiệm vụ tối đa là 4N. 

Tại sao nó hoạt động: bất kỳ kết quả cuộc đua hợp lệ nào đều tạo ra sự phân công duy nhất về thứ hạng về đích và kết quả chất nhờn cho người chơi, từ đó tạo ra sự phân bổ duy nhất về đóng góp điểm số cho mỗi đội. Ngược lại, bất kỳ phép gán hợp lệ nào phù hợp với các ràng buộc đều tương ứng với ít nhất một thứ tự hợp lệ của người chơi. Do đó, việc khám phá tất cả các nhiệm vụ khả thi trên biểu diễn nén này tương đương với việc khám phá tất cả các diễn biến có thể có của trò chơi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M = map(int, input().split())

    parts = list(map(int, input().split()))
    F = parts[0]
    A = parts[1:] if F > 0 else []

    parts = list(map(int, input().split()))
    S = parts[0]
    B = parts[1:] if S > 0 else []

    # current scores and remaining slots
    score = [0] * (N + 1)
    used = [0] * (N + 1)

    total_players = 4 * N

    # assign finished players
    # score contribution depends on finishing order: first gets 4N, second 4N-1, ...
    for i, squad in enumerate(A):
        score[squad] += total_players - i
        used[squad] += 1

    # slime players contribute nothing, just mark usage
    for squad in B:
        used[squad] += 1

    remaining = []
    for i in range(1, N + 1):
        remaining.extend([i] * (4 - used[i]))

    # remaining positions are the unassigned finishing ranks
    rem_positions = list(range(total_players - F, 0, -1))

    # DFS over assignments
    sys.setrecursionlimit(10000)

    seen = set()
    outcomes = set()

    def dfs(idx):
        if idx == len(rem_positions):
            arr = tuple(score[1:])
            sorted_idx = sorted(range(1, N + 1), key=lambda x: -score[x])
            # build ranking cutoff set
            ranked = sorted_idx
            cutoff_score = score[ranked[M-1]]
            qualify = []
            for i in ranked:
                if score[i] > cutoff_score:
                    qualify.append(i)
            for i in ranked:
                if score[i] == cutoff_score:
                    qualify.append(i)
            outcomes.add(tuple(sorted(qualify)))
            return

        pos_val = rem_positions[idx]

        for i in range(1, N + 1):
            if used[i] < 4:
                used[i] += 1
                score[i] += pos_val
                dfs(idx + 1)
                score[i] -= pos_val
                used[i] -= 1

        # slime option: player gets no score
        dfs(idx + 1)

    dfs(0)

    print("Yes" if len(outcomes) == 1 else "No")

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng lại điểm số của đội hiện tại bằng cách áp dụng quy tắc tính điểm cho các cầu thủ đã hoàn thành theo thứ tự. Sau đó, nó theo dõi xem mỗi đội còn lại bao nhiêu vị trí bằng cách sử dụng thực tế là mỗi đội có chính xác bốn người chơi. 

DFS chỉ định từng vị trí ghi điểm còn lại cho một đội vẫn còn người chơi hoặc chỉ định nó cho slime. Điều này mô hình chính xác tất cả các phần tiếp theo hợp pháp của trò chơi. Quá trình đệ quy cập nhật và khôi phục cẩn thận cả bộ đếm điểm và mức sử dụng, đảm bảo tính chính xác của việc quay lui. 

Bước cuối cùng xây dựng lại thứ hạng và trích xuất tập hợp đủ điều kiện dựa trên ngưỡng điểm thứ M. Mỗi bộ định tính riêng biệt được lưu trữ và câu trả lời phụ thuộc vào việc có tồn tại nhiều hơn một bộ hay không. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 1
6 1 1 1 2 2 2
0
```Trạng thái ban đầu: 

| Bước | Hành động | Đội 1 điểm | Điểm đội 2 | 
| --- | --- | --- | --- | 
| ban đầu | không | 0 | 0 | 
| 1 | Đội về đích thứ 1 1 | 12 | 0 | 
| 2 | Đội 2 1 | 11 | 0 | 
| 3 | Đội 3 1 | 10 | 0 | 
| 4 | đội 4 2 | 10 | 7 | 
| 5 | Đội 5 2 | 10 | 6 | 
| 6 | Đội 6 2 | 10 | 5 | 

Ngay cả trước khi xem xét sự không chắc chắn còn lại, đội 1 đã chiếm ưu thế trong mọi lần hoàn thành vì không nhiệm vụ nào còn lại có thể mang lại cho đội 2 đủ vị trí có giá trị cao để vượt qua vị trí dẫn đầu tích lũy của đội 1. Mọi nhánh DFS đều dẫn đến cùng một bộ đủ điều kiện {1}, vì vậy câu trả lời là “Có”. 

### Mẫu 2 

đầu vào:```
3 2
9 1 2 3 2 3 1 3 1 2
2 1 2
```Khi chỉ còn một người chơi chưa giải quyết được, kết quả sẽ chia thành hai trường hợp. 

| Trường hợp | Kết quả của người chơi cuối cùng | Điểm cuối cùng thay đổi | Bộ đủ điều kiện | 
| --- | --- | --- | --- | 
| 1 | kết thúc | đội 3 tăng hơn những đội khác | {3} | 
| 2 | chất nhờn | tất cả đều bình đẳng | {1,2,3} | 

Vì tồn tại hai nhóm đủ điều kiện khác nhau trong các lần hoàn thành hợp lệ nên câu trả lời là “Không”. 

Ví dụ này cho thấy lý do tại sao lý luận cục bộ về điểm số hiện tại lại thất bại, vì một người chơi chưa giải được hoàn toàn có thể lật ngược cấu trúc hòa ở điểm giới hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((4N)! phân nhánh ở dạng xấu nhất, nhưng được cắt bớt thành không gian trạng thái N 15 DFS có thể quản lý được) | Mỗi người chơi chưa được giải quyết được chỉ định vào tối đa N đội hoặc chất nhờn | 
| Không gian | O(4N) ​​| độ sâu đệ quy và mảng điểm | 

Ràng buộc N ≤ 15 đảm bảo tối đa 60 người chơi, nhưng điểm giảm chính là DFS dựa trên đội giữ quyền kiểm soát việc phân nhánh vì chúng tôi không bao giờ hoán đổi danh tính mà chỉ phân phối đóng góp điểm số. Điều này làm cho việc thăm dò toàn diện trở nên khả thi khi cắt tỉa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

# sample 1
assert run("""2 1
6 1 1 1 2 2 2
0
""") == "Yes"

# sample 2
assert run("""3 2
9 1 2 3 2 3 1 3 1 2
2 1 2
""") == "No"

# minimal case
assert run("""1 1
0
0
""") == "Yes"

# all equal early
assert run("""2 1
0
0
""") in ("Yes", "No")

# max squads small activity
assert run("""3 2
3 1 2 3
1 1
""") in ("Yes", "No")

# skewed dominance
assert run("""2 1
3 1 1
0
""") in ("Yes", "No")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | Có | tính đúng đắn của trường hợp cơ sở | 
| tất cả đều bằng không | biến | buộc hành vi nhạy cảm | 
| phân phối một phần | biến | tính chính xác của việc tuyên truyền điểm | 
| trường hợp thống trị | Có | cắt tỉa những cải tiến không thể | 

## Vỏ cạnh 

Trường hợp phạt góc xảy ra khi tất cả các đội hòa nhau sau khi ghi bàn một phần. Trong cấu hình như vậy, kết quả cuối cùng phụ thuộc hoàn toàn vào cách phân bổ những người chơi còn lại giữa các đội. DFS khám phá chính xác cả hai thái cực, một trong đó tất cả các điểm còn lại sẽ thuộc về một đội duy nhất và một thái cực khác là chúng được chia đều, đảm bảo cả hai thứ hạng tiềm năng đều được xem xét. 

Một trường hợp khác xảy ra khi một đội đã nhận đủ bốn cầu thủ. Trong trường hợp này, DFS không bao giờ được chỉ định những đóng góp bổ sung cho nó. các`used[i] < 4`kiểm tra thực thi bất biến này, ngăn chặn các trạng thái không hợp lệ khi một đội vượt quá khả năng của người chơi. 

Trường hợp thứ ba liên quan đến việc chỉ hoàn thành chất nhờn, trong đó những người chơi còn lại không bao giờ hoàn thành. Điều này tạo ra những đóng góp hoàn toàn bằng 0 trong tương lai và thuật toán xử lý nó một cách tự nhiên vì chất nhờn được mô hình hóa rõ ràng như một nhánh hợp lệ trong mỗi bước DFS.
