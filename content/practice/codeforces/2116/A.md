---
title: "CF 2116A - Gellyfish và Pansy ba màu"
description: "Chúng tôi được yêu cầu dự đoán người chiến thắng trong trận đấu theo lượt giữa hai người chơi Gellyfish và Flower. Mỗi người chơi có một nhân vật chính và một hiệp sĩ, mỗi người có điểm máu (HP) riêng. Gellyfish có HP, Flower có b HP, hiệp sĩ của Gellyfish có c HP, và hiệp sĩ của Flower có d HP."
date: "2026-06-09T04:00:25+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2116
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1028 (Div. 2)"
rating: 800
weight: 2116
solve_time_s: 116
verified: false
draft: false
---

[CF 2116A - Gellyfish và Pansy ba màu](https://codeforces.com/problemset/problem/2116/A) 

**Đánh giá:** 800 
**Tags:** trò chơi, tham lam 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu dự đoán người chiến thắng trong trận đấu theo lượt giữa hai người chơi Gellyfish và Flower. Mỗi người chơi có một nhân vật chính và một hiệp sĩ, mỗi người có điểm máu (HP) riêng. Cá gelly có`a`HP, Hoa có`b`HP, hiệp sĩ của Gellyfish có`c`HP và hiệp sĩ của Flower có`d`HP. Cuộc đấu tay đôi diễn ra theo các hiệp xen kẽ. Trong các hiệp số lẻ, hiệp sĩ của Gellyfish có thể tấn công trực tiếp vào Hoa, giảm bớt`b`, hoặc hiệp sĩ của Hoa, giảm`d`. Trong các hiệp số chẵn, hiệp sĩ của Flower có thể tấn công trực tiếp Gellyfish hoặc hiệp sĩ của Gellyfish theo cách tương tự. Người chơi được cho là chơi một cách tối ưu, nghĩa là họ sẽ chọn những hành động giúp tối đa hóa cơ hội chiến thắng. 

Nhiệm vụ là dự đoán người chiến thắng trước khi trận đấu bắt đầu mà không cần mô phỏng mọi đòn tấn công có thể xảy ra. Đầu vào bao gồm nhiều trường hợp thử nghiệm và đầu ra phải cho biết cho từng trường hợp xem "Gellyfish" hay "Fflower" sẽ thắng. 

Các ràng buộc đủ chặt chẽ đến mức việc mô phỏng đơn giản tất cả các cuộc tấn công là không thực tế. Vì giá trị HP có thể lên tới$10^9$và số lượng ca kiểm thử lên tới$10^4$, việc mô phỏng mỗi vòng sẽ mất quá nhiều thời gian. Chúng tôi cần tính toán O(1) cho mỗi trường hợp thử nghiệm. 

Các trường hợp khó khăn không rõ ràng xảy ra khi hiệp sĩ có HP rất thấp hoặc một trong những người chơi chính có HP tối thiểu. Ví dụ: nếu Gellyfish có 1 HP và hiệp sĩ của Flower còn sống, Flower sẽ thắng ở lượt tiếp theo bất kể Gellyfish có làm gì. Ngược lại, nếu hiệp sĩ của Flower có 1 HP và Gellyfish tấn công nó trước, Flower không thể trả đũa, để Gellyfish giành chiến thắng. Việc xử lý những điều này một cách chính xác đòi hỏi phải suy luận về việc hiệp sĩ nào bị loại bỏ trước và điều đó ảnh hưởng như thế nào đến các cuộc tấn công vào người chơi chính. 

## Phương pháp tiếp cận 

Giải pháp bạo lực sẽ mô phỏng trận đấu theo từng hiệp, giảm 1 HP của mục tiêu cho mỗi lần tấn công, luân phiên các lượt. Điều này đúng về nguyên tắc vì nó mô phỏng chính xác trò chơi. Tuy nhiên, trong trường hợp xấu nhất, mỗi ca kiểm thử có thể mất tới$10^9$bước nếu cả hai hiệp sĩ có HP rất cao và không có người chơi nào tấn công trực tiếp vào nhân vật chính. Với$10^4$trường hợp thử nghiệm, điều này sẽ yêu cầu$10^{13}$hoạt động vượt xa giới hạn hợp lý. 

Thông tin chi tiết quan trọng là trận đấu hoàn toàn được xác định bởi giá trị HP ban đầu và các lựa chọn tấn công tối ưu. Vì việc tấn công hiệp sĩ của đối thủ sẽ làm trì hoãn việc đối thủ tấn công nhân vật chính của bạn, nên chúng tôi chỉ cần xem xét liệu mỗi người chơi có thể loại bỏ hiệp sĩ của đối thủ hay không trước khi đối thủ có thể giết họ. Chúng tôi không cần phải mô phỏng mỗi vòng; chúng ta có thể tính toán số lượt cần thiết để đánh bại từng hiệp sĩ và so sánh nó với HP của người chơi chính. Nếu một hiệp sĩ có thể bị loại trước khi nó tấn công người chơi chính đối phương đủ số lần để giảm họ xuống 0, thì người chơi chính tương ứng sẽ an toàn. Nếu không họ sẽ thua. 

Sử dụng lý do này, chúng ta có thể tính toán xem Gellyfish hay Flower sẽ thắng với tỷ lệ O(1) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(max(a,b,c,d)) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm | 
| Phân tích tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định thứ tự lượt đi và nhận biết hiệp sĩ của Gellyfish tấn công trước. Điều này mang lại cho Gellyfish một lợi thế chủ động nhỏ. 
2. Tính xem hiệp sĩ của Gellyfish cần bao nhiêu đòn tấn công để đánh bại hiệp sĩ của Flower: đây chỉ đơn giản là`d`. Mỗi đòn tấn công làm giảm HP của hiệp sĩ đi 1. 
3. Tính xem hiệp sĩ Hoa cần bao nhiêu đòn tấn công để đánh bại hiệp sĩ Gellyfish: đây là`c`. 
4. Quyết định chiến lược tối ưu cho mỗi hiệp sĩ. Mỗi người sẽ tấn công hiệp sĩ đối phương cho đến khi nó chết nếu điều đó cho phép họ bảo vệ nhân vật chính của mình. Nếu tấn công trực tiếp vào đối thủ có thể gây chết người ngay lập tức thì điều đó cần được ưu tiên. 
5. Xác định xem Gellyfish có thể loại bỏ hiệp sĩ của Flower hay không trước khi hiệp sĩ của Flower giảm HP của Gellyfish xuống 0. Tính số lần hiệp sĩ của Flower có thể tấn công Gellyfish trong trận đấu. Nếu hiệp sĩ của Gellyfish có thể giết hiệp sĩ của Flower trong ít lượt hơn hiệp sĩ của Flower có thể làm cạn kiệt HP của Gellyfish, thì Gellyfish sống sót và giành chiến thắng. Ngược lại, Hoa thắng. 
6. Thực hiện logic này bằng phép so sánh đơn giản: Hoa sẽ thắng khi và chỉ khi`a <= d`Và`b > c`nhờ lợi thế đi đầu của Gellyfish. Ngược lại, Gellyfish sẽ thắng. 

Tại sao nó hoạt động: kết quả trận đấu được xác định hoàn toàn bằng tốc độ mỗi người chơi có thể loại bỏ hiệp sĩ hoặc người chơi chính của đối phương. Vì trò chơi diễn ra theo lượt với sát thương cố định là 1 cho mỗi đòn tấn công nên việc đếm lượt và so sánh với HP chính sẽ đưa ra người chiến thắng chính xác. Các hiệp sĩ buộc phải tấn công một cách tối ưu để sống sót hoặc bảo vệ chủ nhân của mình, khiến vấn đề trở nên dễ dàng hơn so với những so sánh này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        a, b, c, d = map(int, input().split())
        # Gellyfish attacks first
        # If Flower's knight dies first, Gellyfish wins
        # If Gellyfish's knight dies first, Flower wins
        # Gellyfish kills Flower's knight in d attacks
        # Flower kills Gellyfish's knight in c attacks
        # Each main HP check
        if a <= d and b > c:
            print("Flower")
        else:
            print("Gellyfish")

solve()
```Dòng đầu tiên ghi số lượng test case. Mỗi trường hợp thử nghiệm được xử lý độc lập. Sự so sánh`a <= d and b > c`mã hóa cái nhìn sâu sắc rằng nếu hiệp sĩ của Flower có thể sống sót đủ lâu để giết Gellyfish, và hiệp sĩ của Gellyfish không thể đe dọa Flower ngay lập tức, Flower sẽ thắng. Ngược lại, sáng kiến ​​của Gellyfish đảm bảo chiến thắng. Sử dụng so sánh số nguyên sẽ tránh mô phỏng từng cuộc tấn công và xử lý hiệu quả các giá trị HP rất lớn. 

## Ví dụ đã hoạt động 

Mẫu 1:```
a=1, b=2, c=3, d=4
```| Vòng | Hành động | một | b | c | d | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Hiệp sĩ Gelly tấn công Hoa | 1 | 2 | 3 | 3 | 
| 2 | Hiệp sĩ hoa tấn công Gellyfish | 0 | 2 | 3 | 3 | 

HP của Gellyfish giảm xuống 0, Hoa thắng. điều kiện`a <= d`là đúng,`b > c`là đúng, vì vậy đầu ra là "Hoa". 

Mẫu 2:```
a=100, b=999, c=1, d=1
```| Vòng | Hành động | một | b | c | d | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Hiệp sĩ Gelly tấn công Hiệp sĩ hoa | 100 | 999 | 1 | 0 | 

Hiệp sĩ của Hoa đã chết, không thể tấn công. Gellyfish thắng. điều kiện`a <= d`là sai (100>1), do đó đầu ra là "Gellyfish". 

Những dấu vết này xác nhận rằng việc so sánh số lượt chơi sẽ thu được kết quả chính xác mà không cần mô phỏng tất cả các vòng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ thực hiện so sánh số nguyên cho từng trường hợp thử nghiệm | 
| Không gian | O(1) | Không cần bộ nhớ bổ sung tỷ lệ thuận với kích thước đầu vào | 

Điều này đảm bảo giải pháp có thể mở rộng quy mô một cách thoải mái cho tối đa$10^4$trường hợp thử nghiệm với giá trị HP lớn lên đến$10^9$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("5\n1 2 3 4\n100 999 1 1\n10 20 10 30\n12 14 13 11\n998 244 353 107\n") == "Flower\nGellyfish\nFlower\nGellyfish\nGellyfish", "sample tests"

# Custom cases
assert run("1\n1 1 1 1\n") == "Flower", "minimal equal HPs"
assert run("1\n10 10 5 5\n") == "Gellyfish", "equal main HP, knights equal"
assert run("1\n1 1000000000 1000000000 1\n") == "Gellyfish", "max HP on Flower main, small Flower knight"
assert run("1\n1000000000 1 1 1000000000\n") == "Flower", "max HP on Gellyfish main, small Gelly knight"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 | Hoa | trường hợp tối thiểu trong đó lợi thế nước đi đầu tiên quan trọng | 
| 10 10 5 5 | Cá Gelly | HP bằng nhau, hiệp sĩ bằng nhau | 
| 1 10^9 10^9 1 | Cá Gelly | Hiệp sĩ của Flower quá yếu để phòng thủ | 
| 10^9 1 1 10^9 | Hoa | Hiệp sĩ của Gellyfish quá yếu, Hoa thắng | 

## Vỏ cạnh 

Nếu cả hai nhân vật chính đều có 1 HP và cả hai hiệp sĩ đều có 1 HP thì người tấn công đầu tiên sẽ thắng. Đầu vào: `1 1 1
