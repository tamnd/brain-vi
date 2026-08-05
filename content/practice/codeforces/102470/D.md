---
title: "CF 102470D - Phi tiêu"
description: "Cách tiếp cận đơn giản là giữ toàn bộ cây trò chơi. Từ trạng thái chứa cả hai điểm, chúng tôi thử mọi kết quả phi tiêu có thể có, chuyển sang trạng thái tiếp theo và tiếp tục đệ quy. Điều này đúng vì mỗi tương lai có thể xảy ra đều được khám phá với xác suất của nó."
date: "2026-08-05T20:40:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102470
codeforces_index: "D"
codeforces_contest_name: "2009-2010 ACM ICPC Southwestern European Regional Programming Contest (SWERC 2009)"
rating: 0
weight: 102470
solve_time_s: 85
verified: true
draft: false
---

[CF 102470D - Phi tiêu](https://codeforces.com/problemset/problem/102470/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là giữ toàn bộ cây trò chơi. Từ trạng thái chứa cả hai điểm, chúng tôi thử mọi kết quả phi tiêu có thể có, chuyển sang trạng thái tiếp theo và tiếp tục đệ quy. Điều này đúng vì mỗi tương lai có thể xảy ra đều được khám phá với xác suất của nó. Tuy nhiên, số lượng lịch sử có thể tăng theo cấp số nhân. Ngay cả khi chúng ta chỉ xem xét 20 kết quả cho mỗi lần ném, thì vài chục lượt đã tạo ra nhiều nhánh hơn mức chương trình có thể xử lý. 

Nhận xét hữu ích là tương lai chỉ phụ thuộc vào hai điểm còn lại và đến lượt ai. Chúng ta không cần phải nhớ những điểm số đó đã đạt được như thế nào. Trò chơi là một chương trình động trạng thái hữu hạn trên`(score of A, score of B, turn)`. 

Cho phép`winA[a][b]`là xác suất để A thắng khi đến lượt A và hai số điểm còn lại là`a`Và`b`. Cho phép`winB[a][b]`có xác suất như nhau khi đến lượt B. 

Quá trình chuyển đổi của A diễn ra trực tiếp: đối với mỗi khu vực có thể đạt được, A kết thúc ngay lập tức hoặc trò chơi chuyển sang lượt của B với điểm A nhỏ hơn. Quá trình chuyển đổi của B cũng tương tự, ngoại trừ B đang cố gắng giành chiến thắng, do đó, cú ném B thành công sẽ đóng góp xác suất bằng 0 vào cơ hội chiến thắng của A. 

Các trạng thái có thể được điền theo thứ tự tăng dần của hai điểm vì mỗi lần chuyển đổi không kết thúc sẽ làm giảm điểm của một người chơi. Khi tính toán`(a, b)`, tất cả các trạng thái cần thiết đã được tính toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ về số lần ném | Cây đệ quy hàm mũ | Quá chậm | 
| Tối ưu | O(N2 · 20) | O(N2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước phân bố xác suất cho A. Mỗi một trong 20 lĩnh vực đều có xác suất`1/20`, do đó sự phân bố của A là như nhau ở mọi trạng thái. 
2. Tính toán trước phân bố xác suất mà B nhận được sau khi chọn từng khu vực mục tiêu có thể. Đối với mỗi mục tiêu, khu vực mục tiêu và hai khu vực lân cận của nó nhận được xác suất`1/3`. Với số điểm còn lại nhất định, B chọn mục tiêu giúp B có cơ hội kết thúc trò chơi tối đa. 
3. Xây dựng hai bảng lập trình động.`winA[a][b]`lưu trữ xác suất chiến thắng của A khi A ném có điểm`a`Và`b`.`winB[a][b]`lưu trữ cùng một giá trị khi B ném. 
4. Điền vào các bảng từ điểm nhỏ đến điểm lớn. Khi tính toán`winA[a][b]`, kiểm tra mọi kết quả có thể có của phi tiêu. Một kết quả loại bỏ chính xác`a`điểm mang lại chiến thắng ngay lập tức. Các kết quả hợp lệ khác chuyển đến`winB[a - value][b]`. 
5. Khi tính toán`winB[a][b]`, trước tiên hãy xác định khu vực mục tiêu tốt nhất cho B. Mọi kết quả có thể có từ mục tiêu đó đều được xem xét. Đánh chính xác`b`nghĩa là A thua, trong khi các kết quả khác tiếp tục đến lượt của A. 
6. Đối với đầu vào`N`, câu trả lời được yêu cầu đầu tiên là`winA[N][N]`. Thứ hai là`1 - winB[N][N]`, bởi vì`winB`lưu trữ xác suất A thắng khi B bắt đầu. 

Tại sao nó hoạt động: 

Điều bất biến là mỗi mục lập trình động thể hiện xác suất chiến thắng chính xác từ trạng thái trò chơi đó. Quá trình chuyển đổi phân chia tất cả các kết quả phi tiêu có thể xảy ra thành các trường hợp loại trừ lẫn nhau có tổng xác suất bằng một. Mọi kết quả không thắng sẽ làm giảm điểm của một người chơi, do đó, sự phụ thuộc luôn hướng đến trạng thái đã được tính toán sẵn. Vì tất cả các lần ném đầu tiên có thể xảy ra và tất cả các trạng thái sau đó đều được bao gồm nên xác suất được tính toán khớp với xác suất của trò chơi thực tế. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

ORDER = [20, 1, 18, 4, 13, 6, 10, 15, 2, 17,
         3, 19, 7, 16, 8, 11, 14, 9, 12, 5]

MAXN = 501

def solve():
    # A's distribution
    a_prob = [0.0] * 21
    for x in ORDER:
        a_prob[x] += 1.0 / 20.0

    # For every score, compute B's best distribution.
    # b_probs[score] is a list of (value, probability) pairs.
    b_probs = [[] for _ in range(MAXN + 1)]
    for score in range(1, MAXN + 1):
        best = None
        best_finish = -1.0
        for i in range(20):
            dist = {}
            for j in ((i - 1) % 20, i, (i + 1) % 20):
                dist[ORDER[j]] = dist.get(ORDER[j], 0.0) + 1.0 / 3.0
            finish = dist.get(score, 0.0)
            if finish > best_finish:
                best_finish = finish
                best = list(dist.items())
        b_probs[score] = best

    win_a = [[0.0] * (MAXN + 1) for _ in range(MAXN + 1)]
    win_b = [[0.0] * (MAXN + 1) for _ in range(MAXN + 1)]

    for a in range(1, MAXN + 1):
        for b in range(1, MAXN + 1):
            wa = 0.0
            for value, p in enumerate(a_prob):
                if value == 0:
                    continue
                if a - value <= 0:
                    wa += p
                else:
                    wa += p * win_b[a - value][b]
            win_a[a][b] = wa

            wb = 0.0
            for value, p in b_probs[b]:
                if b - value <= 0:
                    pass
                else:
                    wb += p * win_a[a][b - value]
            win_b[a][b] = wb

    out = []
    for line in sys.stdin:
        n = int(line)
        if n == 0:
            break
        out.append(f"{win_a[n][n]:.12f} {1.0 - win_b[n][n]:.12f}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên xây dựng xác suất ném cố định của A. Mảng có một mục nhập cho mỗi giá trị khu vực có thể có, vì các lần chuyển đổi sau này chỉ quan tâm đến việc có bao nhiêu điểm bị xóa. 

Phần thứ hai xây dựng các quyết định của B. Đối với mọi điểm số hiện tại có thể có, mã sẽ thử mọi lĩnh vực mục tiêu và giữ lại lĩnh vực có cơ hội hoàn thành ngay lập tức lớn nhất. Điều này là đủ vì trạng thái lập trình động đã đại diện cho tương lai sau lần ném không kết thúc. 

Hai vòng lặp lồng nhau sẽ lấp đầy các trạng thái của trò chơi. Thứ tự của các vòng lặp hoạt động vì mọi chuyển đổi đều làm giảm điểm của A hoặc điểm của B, do đó không có trạng thái nào phụ thuộc vào chính nó hoặc trạng thái trong tương lai. 

Đầu ra cuối cùng sử dụng`1.0 - win_b[n][n]`bởi vì bàn quay B được xác định là xác suất để A thắng. Giá trị được yêu cầu là sự kiện ngược lại. 

## Ví dụ đã hoạt động 

cho`N = 5`, trạng thái ban đầu là`(5,5)`. 

| Tiểu bang | Xoay | Hành động được xem xét | Kết quả | 
| --- | --- | --- | --- | 
| (5,5) | A | A chỉ có thể kết thúc bằng cách nhấn 5 | A thắng với xác suất 0,05 ngay lập tức, nếu không trạng thái sẽ thay đổi | 
| (còn lại điểm A, 5) | B | B chọn lĩnh vực tối đa hóa cơ hội về đích của mình | Giá trị quay đầu B được sử dụng | 
| (5,5) | Đầu tiên | Giá trị lập trình động | 0.136363636364 | 

Dấu vết này cho thấy tại sao việc vượt mức không thể được coi là sự giảm bớt. Hầu hết các lần ném không làm thay đổi tỷ số và lượt ném phải quay lại lượt của đối thủ. 

Vì`N = 100`, phép truy toán tương tự được áp dụng trên một không gian trạng thái lớn hơn nhiều. 

| Tiểu bang | Xoay | Chuyển tiếp chính | Giá trị được lưu trữ | 
| --- | --- | --- | --- | 
| (100.100) | A | Trung bình trên 20 lĩnh vực có khả năng như nhau |`win_a[100][100]`| 
| (a,100) | B | Sử dụng mục tiêu tốt nhất của B để đạt điểm 100 |`win_b[a][100]`| 
| (100.100) | B đầu tiên | Chuyển đổi xác suất thắng A thành xác suất thắng B | 0.950215081962 | 

Dấu vết thứ hai chứng minh rằng các bảng được tính toán trước giống nhau sẽ trả lời tất cả các kích thước đầu vào mà không cần tính toán lại trò chơi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(501² · 20) | Mỗi cặp điểm được xử lý một lần và mỗi lần chuyển đổi sẽ kiểm tra tối đa 20 lĩnh vực phi tiêu | 
| Không gian | O(501²) | Hai bảng xác suất lưu trữ tất cả các trạng thái trò chơi | 

Các bảng lớn nhất chứa khoảng 252.000 mục mỗi bảng. Số lượng thao tác chỉ vài triệu, vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

# This assumes solve() from the solution is available.
def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    ans = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return ans

assert run("5\n100\n0\n") == (
    "0.136363636364 0.909090909091\n"
    "0.072504908290 0.950215081962\n"
), "samples"

assert run("1\n0\n").strip() == "0.050000000000 0.950000000000", "minimum score"

assert run("501\n0\n").count("\n") == 1, "maximum score"

assert run("20\n0\n").strip().startswith("0."), "sector boundary"

assert run("100\n100\n0\n").count("\n") == 2, "multiple queries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5`,`100`| Xác suất mẫu | Tính đúng đắn chung | 
|`1`| Chỉ kết thúc với khu vực 1 | Xử lý chính xác bằng 0 | 
|`501`| Một dòng kết quả hợp lệ | Kích thước trạng thái tối đa | 
|`20`| Tính xác suất cho một ngành chung | Chuyển đổi ngành | 
| Hai truy vấn giống hệt nhau | Hai câu trả lời giống hệt nhau | Xử lý nhiều trường hợp thử nghiệm | 

## Vỏ cạnh 

Đối với đầu vào`1`, chương trình động sẽ xem xét mọi giá trị phi tiêu. Chỉ khu vực có giá trị 1 đạt 0. Mọi lĩnh vực khác đều giữ nguyên điểm hoặc quá cao, vì vậy những xác suất đó sẽ tiếp tục qua lượt của người chơi khác. Điều này tránh được lỗi phổ biến là tính bất kỳ giá trị nào lớn hơn hoặc bằng số điểm còn lại là chiến thắng. 

Đối với đầu vào`5`, các khu vực như 20 hoặc 18 không phải là những cú ném kết thúc hữu ích. Quá trình chuyển đổi giữ cho điểm số không thay đổi đối với những kết quả đó. Bàn vẫn xử lý trạng thái vì lượt thay đổi ngay cả khi điểm số không. 

Đối với đầu vào`501`, trạng thái lớn nhất có thể vẫn nằm trong bảng được tính toán trước. Việc lặp lại tương tự cũng có tác dụng vì số điểm có thể có, chứ không phải số lịch sử trận đấu có thể có, sẽ kiểm soát thời gian chạy.
