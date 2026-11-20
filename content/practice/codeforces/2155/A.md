---
title: "CF 2155A - El fucho"
description: "Chúng ta được yêu cầu tính tổng số trận đấu trong một giải đấu bóng đá loại trực tiếp kép đã được sửa đổi. Trong giải đấu này, các đội bắt đầu ở nhóm chiến thắng. Mỗi vòng, các đội trong nhóm chiến thắng sẽ ghép đôi và thi đấu."
date: "2026-06-08T00:29:46+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2155
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1056 (Div. 2)"
rating: 800
weight: 2155
solve_time_s: 71
verified: true
draft: false
---

[CF 2155A - El fucho](https://codeforces.com/problemset/problem/2155/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính tổng số trận đấu trong một giải đấu bóng đá loại trực tiếp kép đã được sửa đổi. Trong giải đấu này, các đội bắt đầu ở nhóm chiến thắng. Mỗi vòng, các đội trong nhóm chiến thắng sẽ ghép đôi và thi đấu. Đội thua trong các trận đấu vòng bảng sẽ được xếp vào nhóm thua, còn đội thắng ở lại. Các đội ở nhóm thua cũng bắt cặp và thi đấu. Đội thua trong các trận đấu vòng bảng của đội thua sẽ bị loại và đội thắng vẫn ở trong bảng đó. Quá trình này lặp lại cho đến khi mỗi nhóm chỉ còn lại một đội, sau đó các đội sẽ đối đầu trong trận đấu cuối cùng để xác định nhà vô địch chung cuộc. 

Đầu vào cung cấp một số trường hợp thử nghiệm, mỗi trường hợp chỉ định số lượng nhóm$n$. Đầu ra phải là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm: tổng số trận đấu đã diễn ra trong giải đấu đó. 

Những hạn chế là khiêm tốn.$n$dao động từ 2 đến 500 và có thể có tới 100 trường hợp thử nghiệm. Điều này cho phép các giải pháp tuyến tính trong$n$mà không có nguy cơ hết thời gian chờ, vì ngay cả một giải pháp bậc hai cũng sẽ chạy tối đa$500^2 \cdot 100 = 25,000,000$hoạt động ở ranh giới nhưng có thể quản lý được. 

Khía cạnh không rõ ràng là nhận ra rằng kết quả trận đấu chính xác không quan trọng khi tính các trận đấu. Ví dụ, với$n=3$, có thể có một đội chưa ghép đôi trong nhóm thắng cuộc và đội đó sẽ đi tiếp vào vòng tiếp theo mà không cần thi đấu. Việc đếm các trận đấu bằng cách mô phỏng các cặp đấu một cách cẩn thận sẽ mang lại câu trả lời đúng, trong khi một công thức đơn giản bỏ qua các đội không ghép đôi có thể tạo ra lỗi sai từng đội một. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng từng vòng đấu của giải đấu, duy trì hai danh sách: nhóm thắng và nhóm thua. Mỗi vòng, hãy ghép các đội trong mỗi nhóm, tăng số trận đấu cho mỗi cặp và chuyển đội thua từ đội thắng sang đội thua và từ đội thua sang bị loại. Điều này sẽ tính toán chính xác tổng số trận đấu nhưng yêu cầu thực hiện cẩn thận để xử lý số lượng đội lẻ trong một nhóm. Vì$n \le 500$mô phỏng này là khả thi, nhưng cái nhìn sâu sắc dưới đây cho phép một giải pháp đơn giản hơn nhiều. 

Thông tin chi tiết quan trọng là mọi đội ngoại trừ đội vô địch cuối cùng phải thua đúng một lần ở nhóm thua và mọi đội ngoại trừ hai đội cuối cùng cũng phải thua một lần ở nhóm thắng hoặc được ghép đôi và cuối cùng thua. Mỗi trận đấu có đúng một người thua cuộc. Do đó, tổng số trận đấu chỉ đơn giản là$2n - 2$: mỗi cái$n-1$Những người thua cuộc trong giải đấu yêu cầu một trận đấu trong nhóm thua cuộc và mỗi người trong số họ$n-1$Những trận thua ở nhóm thắng trước đó cũng cần phải đấu, trận cuối cùng giữa hai đội cuối cùng cộng lại chính xác. Quan sát này loại bỏ sự cần thiết phải mô phỏng các vòng riêng lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Hoạt động, đơn giản | 
| Công thức: 2n - 2 | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Tối ưu, được chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc số lượng đội$n$. 
3. Áp dụng công thức$2 \cdot n - 2$để tính tổng số trận đấu. Điều này có hiệu quả vì mỗi đội ngoại trừ đội chiến thắng phải thua tổng cộng hai lần: một lần ở nhóm thắng hoặc nhóm thua và một lần bị loại. Trận đấu cuối cùng đã được tính vào số này. 
4. In kết quả cho từng test. 

Tại sao nó hoạt động: Mỗi trận đấu loại bỏ chính xác một đội khỏi nhóm thắng (xuống nhóm thua) hoặc nhóm thua (loại). Giải đấu kết thúc khi chỉ còn lại một đội, chính xác là như vậy$n-1$sự khử xảy ra. Mỗi lần loại trực tiếp chỉ cần một trận đấu và có một trận đấu bổ sung dành cho trận đối đầu cuối cùng của hai đội cuối bảng. Tổng hợp các trận đấu này mang lại$2n-2$, độc lập với kết quả ghép đôi và trận đấu chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    print(2 * n - 2)
```Mã đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó đọc$n$và áp dụng ngay công thức$2n-2$. Điều này tránh được sự mô phỏng không cần thiết, giữ cả thời gian và không gian ở mức tối thiểu. Sự tinh tế chính là hiểu được lý do tại sao công thức này hoạt động: mỗi đội ngoại trừ đội chiến thắng cuối cùng đóng góp chính xác hai trận đấu vào tổng số. 

## Ví dụ đã hoạt động 

cho$n=2$: 

| Bước | Người chiến thắng | Kẻ thua cuộc | Trận đấu | Giải thích | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 0 | 0 | Hai đội xuất phát ở nhóm thắng | 
| Vòng 1 | 1 | 1 | 1 | Người thắng đấu một trận, kẻ thua đánh kẻ thua | 
| Vòng 2 | 1 | 1 | 2 | Trận thua: Đội thua bị loại, trận chung kết giữa hai đội cuối cùng | 

Tổng số trận đấu = 2, phù hợp với công thức$2*2-2=2$. 

Vì$n=3$: 

| Bước | Người chiến thắng | Kẻ thua cuộc | Trận đấu | Giải thích | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 3 | 0 | 0 | Ba đội xuất phát ở nhóm thắng | 
| Vòng 1 | 2 | 1 | 1 | Ghép đôi hai đội, một đội tạm biệt, một đội thua cuộc | 
| Vòng 2 | 1 | 2 | 2 | Trận thắng, trận thua | 
| Vòng 3 | 1 | 1 | 4 | Trận chung kết giữa đội thắng và đội thua cuối cùng | 

Tổng số trận đấu = 4, phù hợp với công thức$2*3-2=4$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu một phép tính công thức duy nhất | 
| Không gian | O(1) | Chỉ các biến số nguyên được lưu trữ | 

Từ$t \le 100$Và$n \le 500$, việc này cực kỳ nhanh và nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        res.append(str(2*n-2))
    return "\n".join(res)

# Provided samples
assert run("2\n2\n3\n") == "2\n4", "Sample 1 and 2"

# Custom test cases
assert run("1\n4\n") == "6", "4 teams"
assert run("1\n500\n") == "998", "maximum n"
assert run("1\n2\n") == "2", "minimum n"
assert run("1\n5\n") == "8", "odd n > 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 | 6 | Đếm đúng cho kích thước vừa phải | 
| 500 | 998 | Công thức hoạt động ở giới hạn trên | 
| 2 | 2 | Công thức hoạt động ở giới hạn dưới | 
| 5 | 8 | Công thức hoạt động với số lẻ n | 

## Vỏ cạnh 

Tối thiểu$n=2$chứng tỏ rằng công thức tính toán chính xác cho kết quả khớp đầu tiên và kết quả khớp cuối cùng. Tối đa$n=500$cho thấy dù các đội được ghép đôi như thế nào hay đội nào thắng thì tổng số trận đấu vẫn được giữ nguyên$2n-2=998$. Đối với số đội lẻ, việc tạm biệt nhóm thắng không ảnh hưởng đến công thức, vì tổng số trận đấu chỉ phụ thuộc vào việc loại chứ không phụ thuộc vào từng cặp riêng lẻ.
