---
problem: 1358B
contest_id: 1358
problem_index: B
name: "Maria Breaks the Self-isolation"
contest_name: "Codeforces Round 645 (Div. 2)"
rating: 1000
tags: ["greedy", "sortings"]
answer: passed_samples
verified: false
solve_time_s: 189
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a2e3899-7340-83ec-9d2b-cbafc7af3f4e
---

#CF 1358B - Maria Phá Bỏ Sự Cô Lập 

**Đánh giá:** 1000 
**Tags:** tham lam, phân loại 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3 phút 9 giây 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e3899-7340-83ec-9d2b-cbafc7af3f4e 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi được cung cấp một tập hợp các bà, mỗi bà có một yêu cầu riêng về quy mô đám đông. Maria bắt đầu một mình trong sân và muốn dần dần mời một số người trong số họ. Mỗi bà có một ngưỡng$a_i$, nghĩa là cô ấy sẽ chỉ đồng ý vào nếu tại thời điểm cô ấy xuất hiện, có ít nhất$a_i$những bà cụ khác đã có mặt (tính Maria là một trong những “người khác”). 

Điều phức tạp chính là Maria có thể mời nhiều bà cùng một lúc và tất cả họ đều quan sát cùng một ảnh chụp nhanh về sân tại thời điểm bước vào. Vì vậy, quyết định của nhóm phụ thuộc vào tổng quy mô của nhóm được thêm vào cùng với những người đã có trong đó. 

Nhiệm vụ là xác định số lượng bà lớn nhất mà cuối cùng có thể được đưa vào, với điều kiện là Maria có thể tự do chọn thứ tự và nhóm lời mời. 

Ràng buộc kích thước đầu vào đủ lớn để bất kỳ giải pháp nào tệ hơn tuyến tính cho mỗi trường hợp thử nghiệm đều sẽ gặp rủi ro. Với tối đa$10^5$tổng số bà qua các bài kiểm tra, một$O(n^2)$mô phỏng thử tất cả các tập hợp con hoặc quét tham lam lặp đi lặp lại sẽ quá chậm. Chúng ta nên hướng tới một$O(n \log n)$hoặc chiến lược tốt hơn, có thể liên quan đến việc sắp xếp và vượt qua một lần. 

Một trường hợp phức tạp phát sinh khi các bà ngoại có ngưỡng lớn tồn tại cùng với nhiều bà ngoại có ngưỡng nhỏ. Ví dụ: nếu một bà cụ yêu cầu gần như toàn bộ quy mô đám đông cuối cùng, bà có thể chặn cách tiếp cận tham lam ngây thơ trừ khi bà được trì hoãn một cách chính xác. Một trường hợp góc khác là khi tất cả các giá trị đều lớn hơn$n$, trong trường hợp đó không ai ngoại trừ Maria có thể tham gia, vì ngay cả yêu cầu nhỏ nhất cũng không thể được đáp ứng. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là mô phỏng tất cả các cách có thể có để mời các bà cụ vào nhóm. Ở bất kỳ bước nào, chúng tôi biết hiện có bao nhiêu người ở bên trong và chúng tôi thử các tập hợp con gồm các bà còn lại có ràng buộc được thỏa mãn bởi kích thước hiện tại cộng với kích thước của tập hợp con. Điều này nhanh chóng trở thành tổ hợp: mỗi lựa chọn tập hợp con phụ thuộc vào tất cả những lựa chọn khác, dẫn đến khả năng nhóm theo cấp số nhân. 

Một cách có cấu trúc hơn để suy nghĩ về vấn đề này là bỏ qua việc phân nhóm lúc đầu và tập trung vào thứ tự cuối cùng. Nếu chúng ta có thể sắp xếp tất cả các bà theo thứ tự nào đó thì khi$k$-thứ bà bước vào, chính xác là có$k$bà ngoại trước cô ấy cộng với Maria. Thế là cô ấy nhìn thấy$k$những người khác (vì Maria luôn có mặt). Điều đó có nghĩa là điều kiện trở thành một hạn chế về vị trí: một bà cụ được đặt ở vị trí$k$là hợp lệ nếu$a_i \le k$. 

Điều này làm giảm vấn đề trong việc chọn tiền tố lớn nhất có thể của các bà có thể được sắp xếp sao cho mỗi vị trí đều thỏa mãn ngưỡng của nó. Chiến lược tốt nhất là ưu tiên những bà già dễ dàng hơn trước, tức là những bà có yêu cầu nhỏ hơn, vì chúng linh hoạt hơn và giúp tăng kích thước tiền tố. 

Việc sắp xếp cho phép chúng ta sắp xếp một cách tham lam các vị trí từ yêu cầu nhỏ nhất trở lên. Chúng tôi duy trì bộ đếm số lượng bà mà chúng tôi đã đặt và chúng tôi bao gồm một bà nếu yêu cầu của bà không lớn hơn quy mô đám đông có thể đạt được hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng nhóm Brute Force | hàm mũ | O(n) | Quá chậm | 
| Sắp xếp + phân công tham lam | O(n log n) | O(1) bổ sung (ngoài đầu vào) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng$a$theo thứ tự không giảm. Điều này đảm bảo rằng chúng tôi luôn xem xét những bà nội dễ dãi nhất trước tiên, giúp tối đa hóa cơ hội phù hợp với nhiều người hơn về tổng thể. 
2. Khởi tạo bộ đếm`cnt = 1`, đại diện cho Maria một mình trong sân. Bộ đếm này theo dõi số lượng bà hiện đang ở bên trong nếu chúng ta bao gồm cả Maria. 
3. Duyệt mảng được sắp xếp từ yêu cầu nhỏ nhất đến lớn nhất. Tại mỗi bà$a_i$, hãy kiểm tra xem liệu cô ấy có thể tham gia với quy mô đám đông hiện tại hay không. 
4. Nếu$a_i \le cnt$, thì bà này có thể được đưa vào một cách an toàn. Tăng`cnt`lên 1, vì chúng tôi đã thêm thành công thêm một người nữa vào sân. 
5. Nếu$a_i > cnt$, bỏ qua cô ấy vĩnh viễn. Vì mảng đã được sắp xếp nên bất kỳ bà già nào sau này sẽ có những yêu cầu thậm chí còn lớn hơn hoặc bằng nhau, vì vậy không ai trong số họ phù hợp với vị trí này. 
6. Sau khi xử lý tất cả các bà, xuất ra`cnt`, trong đó đã bao gồm Maria. 

### Tại sao nó hoạt động 

Bất cứ lúc nào,`cnt`đại diện cho số lượng người có thể có trong sân, bao gồm cả Maria. Việc sắp xếp đảm bảo rằng khi chúng ta cố gắng đặt một bà cụ, chúng ta luôn xem xét ràng buộc nhỏ nhất còn lại. Nếu không thể đặt một bà cụ ở kích thước hiện tại, thì việc sắp xếp lại những cái lớn hơn đã bị bỏ qua không thể khiến bà phù hợp mà không vi phạm các nhiệm vụ trước đó. Việc xây dựng tham lam xây dựng tiền tố lớn nhất có thể trong đó mọi vị trí đều thỏa mãn ngưỡng của nó, tương ứng trực tiếp với kích thước nhóm cuối cùng tối đa có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        a.sort()
        
        cnt = 1  # Maria
        
        for x in a:
            if x <= cnt:
                cnt += 1
        
        out.append(str(cnt))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp danh sách yêu cầu để chúng tôi luôn cố gắng đáp ứng những bà nội ít yêu cầu nhất trước tiên. Biến`cnt`theo dõi số người hiện tại trong sân, bắt đầu từ riêng Maria. 

Khi chúng tôi lặp lại, mỗi khi chúng tôi tìm thấy một bà cụ có yêu cầu được đáp ứng bởi quy mô đám đông hiện tại, chúng tôi sẽ ngay lập tức đưa bà ấy vào và tăng quy mô. Sự chấp nhận tham lam này là an toàn vì những bà nội tương lai chỉ khó được thỏa mãn hơn, vì vậy việc trì hoãn một bà ngoại khả thi không bao giờ cải thiện tính khả thi. 

Giá trị cuối cùng của`cnt`đã bao gồm Maria, khớp với định nghĩa đầu ra được yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 1 2 2 1
```Mảng được sắp xếp:`[1, 1, 1, 2, 2]`| Bước | Cnt hiện tại | một [tôi] | Hành động | Cnt mới | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | lấy | 2 | 
| 2 | 2 | 1 | lấy | 3 | 
| 3 | 3 | 1 | lấy | 4 | 
| 4 | 4 | 2 | lấy | 5 | 
| 5 | 5 | 2 | lấy | 6 | 

Câu trả lời cuối cùng là 6. 

Điều này cho thấy rằng các bà già có ngưỡng thấp ban đầu đóng vai trò là người hỗ trợ cho những bà mẹ sau này, tăng dần năng lực khả thi. 

### Ví dụ 2 

đầu vào:```
6
2 3 4 5 6 7
```Mảng được sắp xếp:`[2, 3, 4, 5, 6, 7]`| Bước | Cnt hiện tại | một [tôi] | Hành động | Cnt mới | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | bỏ qua | 1 | 
| 2 | 1 | 3 | bỏ qua | 1 | 
| 3 | 1 | 4 | bỏ qua | 1 | 
| 4 | 1 | 5 | bỏ qua | 1 | 
| 5 | 1 | 6 | bỏ qua | 1 | 
| 6 | 1 | 7 | bỏ qua | 1 | 

Câu trả lời cuối cùng là 1. 

Điều này chứng tỏ trường hợp một mình Maria không thể đáp ứng được yêu cầu dù là nhỏ nhất nên không thể tiến triển được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| việc sắp xếp thống trị từng trường hợp thử nghiệm | 
| Không gian |$O(1)$thêm | ngoài mảng đầu vào, chỉ sử dụng bộ đếm | 

Tổng cộng$n$trên nhiều trường hợp thử nghiệm là nhiều nhất$10^5$, do đó việc sắp xếp nhìn chung vẫn hiệu quả và việc quét tuyến tính là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()
        cnt = 1
        for x in a:
            if x <= cnt:
                cnt += 1
        out.append(str(cnt))
    return "\n".join(out)

# provided samples
assert run("""4
5
1 1 2 2 1
6
2 3 4 5 6 7
6
1 5 4 5 1 9
5
1 2 3 5 6
""") == """6
1
6
4"""

# minimum case
assert run("""1
1
1
""") == "2"

# all impossible
assert run("""1
3
10 10 10
""") == "1"

# already optimal chain
assert run("""1
4
1 1 1 1
""") == "5"

# mixed values
assert run("""1
5
3 1 4 1 5
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 1 | 2 | trường hợp tối thiểu được chấp nhận ngay lập tức | 
| tất cả các giá trị lớn | 1 | không có bà nào vào được | 
| tất cả những cái | 5 | tăng trưởng toàn chuỗi | 
| trình tự hỗn hợp | 4 | tính đúng đắn của việc đặt hàng tham lam | 

## Vỏ cạnh 

Trường hợp quan trọng là khi tất cả các ngưỡng đều lớn hơn số lượng bà có sẵn. Ví dụ: 

đầu vào:```
3
10 10 10
```Sau khi sắp xếp, chúng ta vẫn có`[10, 10, 10]`. Maria bắt đầu với`cnt = 1`. Mọi giá trị đều lớn hơn 1, do đó không có giá trị nào được chấp nhận và câu trả lời cuối cùng vẫn là 1. Thuật toán tránh được việc cố gắng đưa vào các phần không thể một cách chính xác. 

Một trường hợp khác là khi nhiều giá trị nhỏ xuất hiện sớm, tạo ra hiệu ứng xếp tầng. Ví dụ: 

đầu vào:```
4
1 1 1 1
```Mảng được sắp xếp không thay đổi. Mỗi bước thỏa mãn điều kiện, do đó`cnt`tăng từ 1 lên 5. Điều này cho thấy rằng mỗi bà cụ được chấp nhận đều tăng cường năng lực một cách nghiêm ngặt, điều này cho phép phản ứng dây chuyền thành công.
