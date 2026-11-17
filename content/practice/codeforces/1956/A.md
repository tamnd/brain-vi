---
title: "CF 1956A - Trò Chơi Của Nene"
description: "Chúng tôi được cung cấp một danh sách ngày càng tăng các vị trí và một quy trình liên tục xóa người chơi khỏi một hàng. Trong mỗi vòng, chúng tôi nhìn vào đội hình hiện tại và cố gắng loại bỏ những người chơi đứng ở các vị trí $a1, a2, ldots, ak$."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "games", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1956
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 939 (Div. 2)"
rating: 800
weight: 1956
solve_time_s: 66
verified: false
draft: false
---

[CF 1956A - Trò chơi của Nene](https://codeforces.com/problemset/problem/1956/A) 

**Đánh giá:** 800 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, cấu trúc dữ liệu, trò chơi, tham lam 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách ngày càng tăng các vị trí và một quy trình liên tục xóa người chơi khỏi một hàng. Ở mỗi vòng, chúng tôi nhìn vào đội hình hiện tại và cố gắng loại bỏ những người chơi đang đứng ở vị trí$a_1, a_2, \ldots, a_k$. Các vị trí này luôn được giải thích tương ứng với đội hình còn lại hiện tại chứ không phải đội hình ban đầu. Nếu vị trí được yêu cầu lớn hơn số lượng người chơi hiện tại, vị trí đó sẽ bị bỏ qua trong vòng đó. Sau khi tất cả các thao tác xóa có thể có trong một vòng được thực hiện đồng thời, chúng tôi lặp lại quy trình trên dòng rút gọn. Trò chơi dừng lại khi hết một vòng mà không có lượt xóa nào và phần còn lại được tuyên bố là người chiến thắng. 

Đối với mỗi truy vấn$n_i$, chúng ta được hỏi: bắt đầu từ$n_i$người chơi, cuối cùng có bao nhiêu người sống sót. 

Các hạn chế là nhỏ theo một cách rất cụ thể. Cả hai$k$và mỗi$a_i$nhiều nhất là 100 và mỗi truy vấn$n_i$cũng nhiều nhất là 100. Điều này ngay lập tức gợi ý rằng hệ thống đủ nhỏ để việc mô phỏng trực tiếp quy trình là khả thi nếu được thực hiện cẩn thận. Tuy nhiên, mô phỏng đơn giản cho mỗi truy vấn vẫn sẽ lãng phí nếu được triển khai không hiệu quả vì mỗi vòng đều liên quan đến việc xóa và quét lặp lại. 

Một điểm tinh tế hơn là quá trình này mang tính quyết định nhưng chỉ phụ thuộc vào việc lập chỉ mục tương đối, điều này sẽ thay đổi sau mỗi lần xóa. Điều đó khiến chúng ta dễ thực hiện không chính xác nếu chúng ta vô tình coi các chỉ mục là tĩnh hoặc quên rằng thao tác xóa sẽ thay đổi vị trí. 

Một số trường hợp đặc biệt quan trọng: 

Nếu$n < a_1$, thì sẽ không có việc xóa nào xảy ra, vì vậy tất cả người chơi đều là người chiến thắng. Ví dụ, nếu$a = [3, 5]$Và$n = 2$, câu trả lời phải là 2. Việc triển khai ngây thơ cố gắng truy cập các chỉ mục một cách mù quáng có thể cố gắng xóa sai hoặc thu nhỏ mảng không chính xác. 

Nếu nhiều$a_i$vượt quá kích thước hiện tại, chỉ những giá trị nằm trong giới hạn mới được xem xét mỗi vòng. Ví dụ, nếu$a = [2, 4, 6]$và kích thước hiện tại là 3, chỉ xóa vị trí 2. 

Cuối cùng, một cách tiếp cận bất cẩn có thể cố gắng tính toán trước số lượng bị loại bỏ mỗi vòng với giả định các chỉ số cố định. Điều đó không thành công vì việc loại bỏ trong một vòng diễn ra đồng thời và thay đổi việc lập chỉ mục trong tương lai theo cách phi tuyến tính. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: duy trì rõ ràng danh sách người chơi và mô phỏng các vòng đấu. Trong mỗi vòng, chúng tôi rà soát danh sách và đánh dấu vị trí$a_1, a_2, \ldots, a_k$vẫn còn hiệu lực trong danh sách hiện tại. Sau đó, chúng tôi xóa chúng và lặp lại cho đến khi việc vượt qua đầy đủ không gây ra tình trạng xóa nào. 

Điều này hoạt động chính xác vì nó tuân theo định nghĩa quy trình. Vấn đề là hiệu suất: trong mỗi vòng chúng tôi có thể quét tới 100 phần tử và chúng tôi có thể lặp lại tới$O(n)$vòng trong trường hợp xấu nhất. Sang$q$các truy vấn, điều này trở nên nhỏ một cách thoải mái đối với các ràng buộc lên tới 100, nhưng nó vẫn nặng nề không cần thiết và có nguy cơ kém hiệu quả trong việc triển khai nếu thực hiện xây dựng lại danh sách mỗi vòng. 

Quan sát quan trọng là không gian trạng thái rất nhỏ. Số lượng người chơi tối đa là 100, vì vậy thay vì mô phỏng độc lập từng truy vấn, chúng tôi có thể tính toán trước câu trả lời cho tất cả$n \in [1, 100]$. Mỗi mô phỏng chạy trong một hệ thống giới hạn và tạo ra các kết quả xác định. Khi chúng tôi tính toán kết quả cho tất cả các kích thước ban đầu có thể có một lần, chúng tôi sẽ trả lời từng truy vấn trong O(1). 

Chúng tôi cũng có thể cải thiện bản thân mô phỏng một chút: thay vì liên tục xây dựng lại danh sách, chúng tôi duy trì một mảng boolean hoặc sử dụng danh sách và chỉ xây dựng lại khi cần. Vì mọi thứ đều nhỏ nên sự rõ ràng quan trọng hơn việc tối ưu hóa vi mô. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu cho mỗi truy vấn |$O(q \cdot n^2)$|$O(n)$| Được chấp nhận (hầu như không có ràng buộc nhỏ) | 
| Tính toán trước cho tất cả$n \le 100$|$O(100^3)$tổng số trường hợp xấu nhất |$O(100)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng quy trình cho từng kích thước ban đầu có thể có từ 1 đến 100 một lần, lưu trữ số lượng người sống sót cuối cùng. 

1. Để bắt đầu cố định$n$, tạo danh sách đại diện cho người chơi$1$bởi vì$n$. Danh sách này chỉ theo dõi sự tồn tại chứ không phải danh tính, vì danh tính không quan trọng. 
2. Thực hiện lặp lại các vòng cho đến khi không có thao tác xóa nào xảy ra trong một vòng. Chúng tôi sử dụng cờ để phát hiện xem có ít nhất một lần xóa xảy ra hay không. 
3. Trong mỗi vòng, lặp qua danh sách các chỉ số đã được sắp xếp$a_1, \ldots, a_k$. Đối với mỗi$a_i$, kiểm tra xem nó có nằm trong kích thước danh sách hiện tại hay không. Nếu có thì đánh dấu vị trí đó để loại bỏ. 
4. Sau khi quét tất cả$a_i$, loại bỏ tất cả các vị trí được đánh dấu cùng một lúc. Tính đồng thời rất quan trọng vì việc loại bỏ các phần tử trước đó sẽ làm thay đổi chỉ số; đánh dấu đầu tiên sẽ tránh được việc xếp tầng không chính xác trong cùng một vòng. 
5. Nếu không có vị trí nào bị loại bỏ trong toàn bộ vòng chơi, hãy kết thúc quá trình. Kích thước danh sách còn lại là câu trả lời cho điều này$n$. 
6. Lưu kết quả này vào một mảng để mỗi truy vấn được trả lời trong thời gian không đổi. 

Lý do cốt lõi điều này hoạt động là vì một cố định$n$, quá trình này mang tính xác định và độc lập với các truy vấn. Khi chúng tôi tính toán trạng thái cuối cho mỗi$n$, việc sử dụng lại là an toàn vì các truy vấn không tương tác. 

### Tại sao nó hoạt động 

Điều bất biến chính là mỗi vòng hoạt động dựa trên sự hoán vị hiện tại được xác định rõ ràng của những người chơi còn lại và trạng thái duy nhất quan trọng là số lượng người chơi hiện tại và thứ tự tương đối của họ. Vì quy tắc xóa chỉ phụ thuộc vào vị trí xếp hạng chứ không phụ thuộc vào danh tính nên mô phỏng vẫn duy trì tính chính xác ngay cả khi chúng tôi coi người chơi là chỉ số ẩn danh. Mỗi lần chuyển đổi sẽ giảm nghiêm ngặt số lượng người chơi trừ khi không có chỉ mục hợp lệ nào tồn tại trong$a$, tại thời điểm đó hệ đạt đến một điểm cố định. Điều này đảm bảo sự chấm dứt và tính chính xác của kết quả được lưu trữ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def simulate(n, a):
    players = list(range(n))
    
    while True:
        removed = [False] * len(players)
        any_removed = False

        for idx in a:
            if idx <= len(players):
                removed[idx - 1] = True

        if not any(removed):
            return len(players)

        new_players = []
        for i in range(len(players)):
            if not removed[i]:
                new_players.append(players[i])

        players = new_players

def main():
    t = int(input())
    
    # Precompute answers for all n up to 100
    # We'll recompute per test case since a changes
    for _ in range(t):
        k, q = map(int, input().split())
        a = list(map(int, input().split()))
        ns = list(map(int, input().split()))

        # cache for this test case
        ans = [0] * 101

        for n in range(1, 101):
            ans[n] = simulate(n, a)

        print(" ".join(str(ans[n]) for n in ns))

if __name__ == "__main__":
    main()
```Việc thực hiện phản ánh mô phỏng được mô tả. Hàm bên trong sẽ xây dựng lại danh sách người chơi sau mỗi vòng bằng cách sử dụng mặt nạ loại bỏ boolean, giúp tránh các lỗi dịch chuyển chỉ mục tinh vi xảy ra khi xóa khỏi danh sách trong khi lặp lại. 

Chúng tôi tính toán trước kết quả cho mỗi trường hợp thử nghiệm vì trình tự$a$thay đổi giữa các trường hợp thử nghiệm. Từ$n$bị giới hạn bởi 100, việc tính toán lại 100 mô phỏng vẫn là chuyện nhỏ. 

Chi tiết tinh tế nhất là lập chỉ mục:$a_i$dựa trên 1, trong khi danh sách Python dựa trên 0. Mặt nạ loại bỏ bản đồ trực tiếp$a_i - 1$vào đúng ô. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi một trường hợp minh họa nhỏ$a = [2, 4]$,$n = 5$. 

### Dấu vết 

| Vòng | Người chơi | Đánh dấu để loại bỏ | Sau khi loại bỏ | Chấm dứt | 
| --- | --- | --- | --- | --- | 
| 1 | [1,2,3,4,5] | 2, 4 | [1,3,5] | Không | 
| 2 | [1,3,5] | 2 không hợp lệ, 4 không hợp lệ | [1,3,5] | Có | 

Vòng thứ hai không loại bỏ được gì nên quá trình dừng lại. 

Điều này chứng tỏ rằng việc chấm dứt phụ thuộc vào việc có bất kỳ chỉ mục nào trong$a$vẫn còn hiệu lực, không phải về việc liệu$a$bản thân nó không trống. 

### Ví dụ thứ hai 

hãy để$a = [1, 2, 3]$,$n = 3$. 

| Vòng | Người chơi | Đã đánh dấu | Sau khi loại bỏ | Chấm dứt | 
| --- | --- | --- | --- | --- | 
| 1 | [1,2,3] | 1,2,3 | [] | Không | 
| 2 | [] | không | [] | Có | 

Điều này cho thấy sự sụp đổ nhanh chóng khi$a$bao gồm đầy đủ danh sách hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \cdot 100 \cdot k \cdot 100)$| Đối với mỗi trường hợp thử nghiệm, chúng tôi mô phỏng tối đa 100 kích thước bắt đầu, mỗi mô phỏng có thể mất tới 100 vòng và mỗi vòng quét tối đa 100 vị trí | 
| Không gian |$O(100)$| Chúng tôi lưu trữ danh sách người chơi hiện tại và một mảng câu trả lời được tính toán trước | 

Với tất cả các giới hạn tối đa là 100, điều này dễ dàng phù hợp với giới hạn. Ngay cả những hoạt động trong trường hợp xấu nhất cũng có khoảng vài triệu bước cơ bản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    def solve():
        input = sys.stdin.readline
        t = int(input())
        for _ in range(t):
            k, q = map(int, input().split())
            a = list(map(int, input().split()))
            ns = list(map(int, input().split()))

            def simulate(n):
                players = list(range(n))
                while True:
                    removed = [False] * len(players)
                    for idx in a:
                        if idx <= len(players):
                            removed[idx - 1] = True
                    if not any(removed):
                        return len(players)
                    players = [players[i] for i in range(len(players)) if not removed[i]]

            ans = []
            for n in ns:
                ans.append(str(simulate(n)))
            print(" ".join(ans))

    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""6
2 1
3 5
5
5 3
2 4 6 7 9
1 3 5
5 4
3 4 5 6 7
1 2 3 4
2 3
69 96
1 10 100
1 1
100
50
3 3
10 20 30
1 10 100
""") == """2
1 1 1
1 2 2 2
1 10 68
50
1 9 9"""

# custom cases
assert run("""1
2 3
2 3
1 2 3
""") == "0 0 0", "all removals"

assert run("""1
1 3
1
1 2 3
""") == "0 0 0", "single removal always kills"

assert run("""1
3 3
5 6 7
1 2 3
""") == "1 2 3", "no valid indices, identity case"

assert run("""1
2 2
1 2
1 2
""") == "0 0", "full collapse"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các lần xóa | tất cả 0 | bị loại hoàn toàn ở vòng đầu tiên | 
| loại bỏ một lần luôn giết chết | tất cả 0 | trường hợp cạnh chỉ tồn tại chỉ số 1 | 
| không có chỉ số hợp lệ | 1 2 3 | hệ thống đạt điểm cố định ngay lập tức | 
| sụp đổ hoàn toàn | 0 0 | hành vi thu hẹp lặp đi lặp lại | 

## Vỏ cạnh 

Khi nào$n$nhỏ hơn giá trị nhỏ nhất trong$a$, mô phỏng sẽ thực hiện toàn bộ quá trình mà không cần loại bỏ ngay lập tức. Ví dụ, với$a = [3, 5]$Và$n = 2$, vòng đầu tiên không có phép xóa hợp lệ nào, vì vậy câu trả lời là đầy đủ 2. Thuật toán xử lý việc này vì mặt nạ loại bỏ vẫn trống và vòng lặp kết thúc ngay lập tức. 

Khi$n$bằng một trong$a_i$, chỉ các chỉ số trong phạm vi mới bị xóa. Ví dụ,$a = [1, 4]$,$n = 3$, chỉ có vị trí 1 bị loại bỏ ở vòng đầu tiên. Việc xóa dựa trên mặt nạ đảm bảo không xảy ra truy cập không hợp lệ và bảo toàn chính xác các phần tử còn lại. 

Khi tất cả các vị trí bị loại bỏ trong một vòng, lần lặp tiếp theo sẽ thấy danh sách trống và dừng ngay lập tức. Việc này được xử lý một cách tự nhiên vì điều kiện vòng lặp sẽ kiểm tra xem có bất kỳ thao tác xóa nào trước khi tiếp tục hay không.
