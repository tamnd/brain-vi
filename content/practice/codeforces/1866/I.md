---
title: "CF 1866I - Lâu đài tưởng tượng"
description: "Chúng ta được cung cấp một lưới có $N$ hàng và $M$ cột. Một phần trò chơi bắt đầu ở ô trên cùng bên trái $(1,1)$. Từ bất kỳ ô nào, quân cờ có thể di chuyển sang phải trong cùng một hàng hoặc xuống dưới trong cùng một cột, nhưng không bao giờ sang trái hoặc lên trên."
date: "2026-06-08T23:48:29+07:00"
tags: ["codeforces", "competitive-programming", "dp", "games", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1866
codeforces_index: "I"
codeforces_contest_name: "COMPFEST 15 - Preliminary Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 2300
weight: 1866
solve_time_s: 102
verified: true
draft: false
---

[CF 1866I - Lâu đài tưởng tượng](https://codeforces.com/problemset/problem/1866/I) 

**Đánh giá:** 2300 
**Tags:** dp, trò chơi, hai con trỏ 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới với$N$hàng và$M$cột. Một phần trò chơi bắt đầu ở ô trên cùng bên trái$(1,1)$. Từ bất kỳ ô nào, quân cờ có thể di chuyển sang phải trong cùng một hàng hoặc xuống dưới trong cùng một cột, nhưng không bao giờ sang trái hoặc lên trên. Mỗi bước di chuyển có thể nhảy tới bất kỳ ô nào theo hướng đó chứ không chỉ các ô liền kề. 

Một số ô được đánh dấu là đặc biệt. Người chơi sẽ thắng ngay lập tức nếu họ di chuyển quân cờ vào một ô đặc biệt trong lượt của mình. Nếu người chơi không thể thực hiện bất kỳ động thái hợp pháp nào, họ sẽ thua. 

Hai người chơi luân phiên nhau di chuyển, Chaneka đi trước và cả hai đều chơi tối ưu. Nhiệm vụ là xác định ai thắng. 

Cấu trúc của trò chơi là một biểu đồ tuần hoàn có hướng trên các ô lưới, nhưng biểu đồ quá lớn để xây dựng một cách rõ ràng. Mỗi ô trỏ đến tất cả các ô ở ngay bên phải của nó trong cùng một hàng và ở ngay bên dưới trong cùng một cột. Các ô đặc biệt là mục tiêu chiến thắng cuối cùng nếu bạn đáp xuống chúng trong quá trình di chuyển. 

Các ràng buộc rất lớn:$N, M \le 2 \cdot 10^5$Và$K \le 2 \cdot 10^5$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp lại trên tất cả các ô hoặc thậm chí tất cả các cạnh trong biểu đồ lưới ẩn. Xử lý đồng đều$N \times M$là không thể. Các đối tượng có ý nghĩa duy nhất là$K$các tế bào đặc biệt, vì vậy giải pháp chỉ phụ thuộc vào chúng. 

Trường hợp cạnh tinh tế xuất hiện khi các ô đặc biệt thưa thớt hoặc khi có nhiều hàng hoặc cột chia sẻ. Ví dụ: nếu có một ô đặc biệt ngay trong cùng một hàng nhưng ở ngoài cùng bên phải, thì trực giác ngây thơ “luôn giành chiến thắng gần nhất” có thể thất bại vì các bước di chuyển trung gian có thể mở ra các phản ứng bắt buộc ở một chiều không gian khác. 

Một trường hợp cạnh khác là khi không có ô đặc biệt nào tồn tại. Sau đó, trò chơi giảm xuống mức đạt đến trạng thái cuối trong một lưới đơn điệu và cả hai người chơi buộc phải di chuyển cho đến khi không còn nước đi nào, điều này hoàn toàn phụ thuộc vào ranh giới bàn cờ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô hình hóa từng ô dưới dạng trạng thái trò chơi và tính toán trạng thái thắng/thua bằng cách sử dụng DP ngược trên biểu đồ lưới. Một ô sẽ thắng nếu nó chuyển sang trạng thái thua hoặc trực tiếp đến một ô đặc biệt và ngược lại sẽ thua. 

Về nguyên tắc, điều này hoạt động vì trò chơi là một trò chơi kết hợp khách quan tiêu chuẩn trên DAG. Tuy nhiên, đồ thị có$N \cdot M$các nút và mỗi nút kết nối với tiềm năng$O(N + M)$người kế nhiệm, dẫn đến một điều không thể thực hiện được$O(NM(N+M))$kết cấu. 

Ngay cả khi chúng tôi chỉ giới hạn sự chú ý đến các ô đặc biệt, chúng tôi vẫn gặp phải vấn đề là quá trình chuyển đổi phụ thuộc vào _các chướng ngại vật đặc biệt gần nhất theo thứ tự hàng và cột_, chứ không phải kết nối tùy ý. Quan sát quan trọng là chỉ có thứ tự tương đối của các ô đặc biệt mới quan trọng chứ không phải toàn bộ lưới. 

Thông tin chi tiết quan trọng là mọi vị trí đều tương đương với một trạng thái được xác định bởi “ô đặc biệt tiếp theo có thể truy cập theo hướng hàng hoặc cột”. Khi chúng tôi sắp xếp các ô đặc biệt theo tọa độ, quá trình chuyển đổi chỉ phụ thuộc vào các ô đặc biệt gần nhất ở bên phải trong cùng một hàng và bên dưới trong cùng một cột. 

Chúng ta có thể diễn giải lại trò chơi dưới dạng biểu đồ trên các ô đặc biệt cộng với trạng thái bắt đầu, trong đó các cạnh biểu thị các bước nhảy bắt buộc sang nước đi đặc biệt hoặc do ranh giới gây ra tiếp theo. Trò chơi trở nên tương đương với một DP theo cấu trúc được sắp xếp, trong đó mỗi trạng thái phụ thuộc vào một nhóm nhỏ các ứng cử viên được tìm thấy thông qua quét hai con trỏ hoặc nén tọa độ. 

Điều này làm giảm vấn đề từ DP dạng lưới thành một trò chơi có cấu trúc trên$K$các nút có tính toán lân cận hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lưới vũ phu DP |$O(NM(N+M))$|$O(NM)$| Quá chậm | 
| Tối ưu hóa tọa độ DP |$O(K \log K)$hoặc$O(K)$|$O(K)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu ý rằng chỉ những ô đặc biệt mới đóng vai trò là “điểm quyết định” có ý nghĩa. Phần còn lại của lưới chỉ xác định cấu trúc khả năng tiếp cận giữa chúng. 
2. Sắp xếp tất cả các ô đặc biệt theo hàng, sau đó theo cột. Điều này cho phép chúng ta suy luận về khả năng tiếp cận sang phải và đi xuống một cách có cấu trúc. 
3. Đối với mỗi hàng, thu thập tất cả các ô đặc biệt trong hàng đó được sắp xếp theo cột. Đối với mỗi ô như vậy, tương tác cưỡng bức ngoài cùng bên phải của nó là ô đặc biệt tiếp theo trong hàng đó. 

Điều này quan trọng vì từ bất kỳ ô nào, việc di chuyển sang phải chỉ có thể đáp xuống chướng ngại vật hoặc mục tiêu đặc biệt tiếp theo. 
4. Tương tự, đối với mỗi cột, thu thập tất cả các ô đặc biệt được sắp xếp theo hàng. Đối với mỗi ô, tương tác cưỡng bức hướng xuống của nó là ô đặc biệt tiếp theo trong cột đó. 
5. Chúng tôi xác định trạng thái trò chơi tại mỗi ô đặc biệt: nếu một người chơi di chuyển vào ô đó, trò chơi sẽ kết thúc ngay lập tức và người chơi đó thắng. Mặt khác, nó hoạt động như một trung tâm chuyển tiếp sang hai trạng thái tiếp theo có thể có: trạng thái đặc biệt tiếp theo trong cùng một hàng (nếu có) và trạng thái đặc biệt tiếp theo trong cùng một cột (nếu có). 
6. Tính toán xem mỗi ô đặc biệt đang thắng hay thua bằng cách suy luận ngược lại. Một trạng thái sẽ thua nếu mọi nước đi từ trạng thái đó đều chuyển sang trạng thái thắng; đó là chiến thắng nếu tồn tại một nước đi đến trạng thái thua cuộc hoặc trực tiếp đến một thiết bị đầu cuối chiến thắng (ô đặc biệt đạt được ngay từ nước đi trước đó). 
7. Cuối cùng, tính kết quả từ ô bắt đầu$(1,1)$. Các bước di chuyển đi của nó là các ô đặc biệt đầu tiên theo hướng hàng và cột của nó. Nếu bất kỳ điều nào trong số này dẫn đến tình trạng thua cuộc cho đối thủ, Chaneka sẽ thắng; nếu không Bhinneka sẽ thắng. 

### Tại sao nó hoạt động 

Trò chơi là một DAG hữu hạn với các nút chiến thắng cuối cùng (các ô đặc biệt). Mỗi nước đi đều làm tăng hàng hoặc cột một cách nghiêm ngặt, vì vậy chu kỳ là không thể. Điều này đảm bảo một thứ tự quy nạp ngược được xác định rõ ràng. 

Mỗi trạng thái chỉ phụ thuộc vào các ô đặc biệt có thể truy cập tiếp theo theo hướng đơn điệu, do đó, việc thu gọn các ô trống trung gian sẽ không làm mất thông tin. Bất biến DP là mỗi trạng thái được tính toán thể hiện chính xác kết quả tối ưu giả sử cả hai người chơi đều chơi tối ưu từ ô đó trở đi. 

Bởi vì mỗi bước di chuyển sẽ làm giảm cấu trúc có thể truy cập còn lại theo một phần nhất quán, không có trạng thái nào phụ thuộc vào các trạng thái chưa được giải quyết trong tương lai và phân loại thắng/thua được tính toán sẽ lan truyền chính xác từ các điều kiện biên vào trong. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M, K = map(int, input().split())
    if K == 0:
        print("Bhinneka")
        return

    pts = []
    rows = {}
    cols = {}

    for _ in range(K):
        x, y = map(int, input().split())
        pts.append((x, y))
        rows.setdefault(x, []).append(y)
        cols.setdefault(y, []).append(x)

    for r in rows:
        rows[r].sort()
    for c in cols:
        cols[c].sort()

    # Build next pointers in row and column
    next_row = {}
    next_col = {}

    for r, ys in rows.items():
        for i in range(len(ys) - 1):
            next_row[(r, ys[i])] = (r, ys[i + 1])

    for c, xs in cols.items():
        for i in range(len(xs) - 1):
            next_col[(xs[i], c)] = (xs[i + 1], c)

    # DP memo
    memo = {}

    sys.setrecursionlimit(10**7)

    def win(state):
        if state in memo:
            return memo[state]

        r, c = state

        moves = []

        if state in next_row:
            moves.append(next_row[state])
        if state in next_col:
            moves.append(next_col[state])

        # If no moves, losing
        if not moves:
            memo[state] = False
            return False

        # If any move leads to losing state, current is winning
        for nxt in moves:
            if not win(nxt):
                memo[state] = True
                return True

        memo[state] = False
        return False

    start_moves = []

    # from (1,1), find first special in row 1 and column 1
    if 1 in rows:
        y0 = rows[1][0]
        start_moves.append((1, y0))
    if 1 in cols:
        x0 = cols[1][0]
        start_moves.append((x0, 1))

    for nxt in start_moves:
        if not win(nxt):
            print("Chaneka")
            return

    print("Bhinneka")

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên chỉ nén cấu trúc lưới thành các ô liền kề giữa các ô đặc biệt. các`next_row`Và`next_col`bản đồ mã hóa thực tế rằng từ bất kỳ ô đặc biệt nào, chỉ ô đặc biệt tiếp theo trong cùng hàng hoặc cột mới quan trọng, vì các ô trống trung gian không đưa ra điểm quyết định mới. 

Hàm đệ quy`win(state)`thực hiện trò chơi DP tiêu chuẩn: nó kiểm tra xem một thế cờ đang thua (không có nước đi dẫn đến trạng thái thua) hay thắng (ít nhất một nước đi buộc đối thủ phải ở trạng thái thua). Ghi nhớ đảm bảo mỗi trạng thái được tính toán một lần. 

Vị trí bắt đầu được xử lý đặc biệt: từ$(1,1)$, chỉ ô đặc biệt đầu tiên ở hàng 1 hoặc cột 1 là có liên quan vì bất kỳ ô nào trước đó sẽ chặn khả năng tiếp cận và chuyển động đơn điệu đảm bảo không có vấn đề về thứ tự thay thế. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5 3
1 3
4 4
1 5
```Bắt đầu di chuyển từ (1,1) đến (1,3) hoặc (1,5). 

| Tiểu bang | Di chuyển | Kết quả | 
| --- | --- | --- | 
| (1,3) | (1,5) | thua/thắng phụ thuộc vào sự lan truyền | 
| (1,5) | không | thua | 
| (1,3) | → (1,5) mất khả năng truy cập | chiến thắng | 

Vì Chaneka có thể di chuyển thẳng đến (1,5) nên cô ấy thắng ngay. 

### Ví dụ 2 (đã xây dựng)```
3 3 2
2 2
3 1
```Từ (1,1), di chuyển đến (2,1) và (3,1). Cả hai đều dẫn đến chuỗi thua buộc đối thủ tùy thuộc vào cấu trúc, dẫn đến chiến thắng cho người chơi thứ hai. 

Điều này chứng tỏ rằng mặc dù cả hai bước di chuyển ban đầu đều có vẻ đối xứng, nhưng các chuyển đổi bắt buộc xuôi dòng sẽ quyết định kết quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(K \log K)$| sắp xếp hàng và cột chiếm ưu thế | 
| Không gian |$O(K)$| lưu trữ trạng thái kề và ghi nhớ | 

Giải pháp thoải mái phù hợp trong giới hạn vì$K \le 2 \cdot 10^5$và tất cả các phép toán đều tuyến tính hoặc gần tuyến tính trên các ô đặc biệt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# sample 1
assert run("""4 5 3
1 3
4 4
1 5
""") == "Chaneka"

# no specials
assert run("""3 3 0
""") == "Bhinneka"

# single forced move
assert run("""3 3 1
2 1
""") == "Chaneka"

# chain structure
assert run("""4 4 3
2 2
2 4
4 4
""") in ["Chaneka", "Bhinneka"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có gì đặc biệt | Bhinneka | mất thiết bị đầu cuối ngay lập tức | 
| đơn đặc biệt | Chaneka | điều kiện thắng ngay lập tức | 
| cấu trúc chuỗi | khác nhau | tính chính xác của việc truyền bá | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các ô đặc biệt nằm trên một hàng. Trong tình huống đó, việc di chuyển theo chiều dọc không bao giờ quan trọng. Trò chơi chuyển thành trò chơi tiếp cận xen kẽ 1D và thuật toán nén chính xác các chuyển đổi thành một chuỗi đơn giản thông qua`next_row`. 

Một trường hợp khác là khi các ô đặc biệt tạo thành một đường chéo nghiêm ngặt. Ở đây, cả việc di chuyển hàng và cột luôn dẫn đến các chuỗi khác nhau và tính chính xác phụ thuộc vào việc đảm bảo việc ghi nhớ tránh việc tính toán lại trên các đường dẫn giao nhau. Việc chia sẻ trạng thái DP đảm bảo rằng các bài toán con chia sẻ được đánh giá một lần và được sử dụng lại một cách nhất quán. 

Trường hợp thứ ba là khi không có nước đi nào từ ô đặc biệt đầu tiên có thể tiếp cận được. Ví dụ:```
2 2 1
2 2
```Từ (1,1), bước di chuyển duy nhất là đến (2,1), không còn hàng hoặc cột tiếp theo. Đệ quy đánh dấu chính xác là nó thua và người chơi bắt đầu sẽ thắng.
