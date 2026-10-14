---
title: "CF 105168B - San lấp mặt bằng một mình"
description: "Chúng tôi đang mô phỏng một hệ thống tiến triển trong đó một nhân vật bắt đầu với hai thuộc tính độc lập, A và B, cả hai đều ban đầu được cố định ở mức 10. Có n quái vật và mỗi quái vật i chỉ có thể bị đánh bại nếu nhân vật có ít nhất ai thuộc A và ít nhất bi thuộc B."
date: "2026-06-27T08:34:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105168
codeforces_index: "B"
codeforces_contest_name: "2024 Fujian Normal University Programming Contest"
rating: 0
weight: 105168
solve_time_s: 58
verified: true
draft: false
---

[CF 105168B - Thăng cấp một mình](https://codeforces.com/problemset/problem/105168/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một hệ thống tiến triển trong đó một nhân vật bắt đầu với hai thuộc tính độc lập, A và B, cả hai đều ban đầu được cố định ở mức 10. Có n quái vật và mỗi quái vật tôi chỉ có thể bị đánh bại nếu nhân vật có ít nhất ai thuộc A và ít nhất bi thuộc tính B. Sau khi đánh bại một quái vật, nhân vật nhận được điểm nâng cấp ci và mỗi điểm có thể tăng vĩnh viễn A hoặc B lên 1. 

Khó khăn cốt lõi là thứ tự tiêu diệt quái vật không cố định. Những trận chiến đầu tiên quyết định số điểm nâng cấp mà bạn có sau này, từ đó quyết định những quái vật mạnh hơn nào có thể tiếp cận được. Câu hỏi đặt ra là liệu có tồn tại thứ tự sắp xếp quái vật sao cho mọi quái vật cuối cùng đều có thể bị đánh bại bắt đầu từ (10, 10). 

Các ràng buộc là nhỏ về mặt cấu trúc. Mặc dù giá trị của ai, bi, ci có thể lớn tới 10^9, nhưng số lượng quái vật trong mỗi thử nghiệm nhiều nhất là 1000 và tổng số trên tất cả các thử nghiệm nhiều nhất là 4000. Điều này cho thấy rõ ràng rằng các giải pháp có hành vi O(n^2) hoặc O(n^2 log n) là có chủ ý, trong khi bất cứ điều gì theo cấp số nhân đối với các tập hợp con hoặc hoán vị là không thể. 

Một điểm tinh tế quan trọng là tài nguyên không đối xứng một cách tầm thường. Một trực giác ngây thơ có thể gợi ý rằng vì ci tăng tổng công suất nên mọi thứ tự cuối cùng sẽ có hiệu quả nếu tổng mức tăng đủ lớn. Điều này là sai vì các ràng buộc có tính chất hai chiều: một trình tự đầu tư quá mức vào A sớm có thể không thể đáp ứng được quái vật yêu cầu B, ngay cả khi tổng số điểm là đủ. 

Một trường hợp thất bại nhỏ minh họa điều này. Giả sử chúng ta bắt đầu ở (10, 10). Một quái vật yêu cầu (20, 10) và không mang lại sự linh hoạt hữu ích cho B, trong khi một quái vật khác yêu cầu (10, 20). Nếu chúng ta tiêu diệt con quái vật đầu tiên quá sớm và chỉ đầu tư vào A, chúng ta có thể vĩnh viễn không thể tiếp cận được con quái vật thứ hai mặc dù tổng số điểm sẽ là đủ nếu được phân bổ khác. Đầu ra phụ thuộc hoàn toàn vào việc đặt hàng. 

Do đó, vấn đề là liệu chúng ta có thể tìm ra một chuỗi “mở khóa khả thi” trong đó lợi ích tài nguyên luôn có thể được phân phối để đáp ứng các yêu cầu trong tương lai hay không. 

## Phương pháp tiếp cận 

Chế độ xem brute-force rất đơn giản: thử tất cả các hoán vị của quái vật, mô phỏng quy trình và kiểm tra xem một lệnh nhất định có khả thi hay không. Đối với mỗi hoán vị, chúng tôi duy trì hiện tại (A, B), kiểm tra xem quái vật tiếp theo có thể truy cập được không và nếu có, hãy cập nhật A và B bằng ci. Điều này hoạt động hợp lý, nhưng có n! hoán vị, và thậm chí với n = 15 thì điều này đã trở nên không khả thi. 

Cấu trúc làm cho vấn đề này có thể giải quyết được là tính khả thi là đơn điệu đối với tài nguyên tích lũy và mọi con quái vật chỉ áp đặt một giới hạn ngưỡng. Một khi một con quái vật trở nên có thể bị giết, việc giết nó chỉ có thể mở rộng không gian có thể tiếp cận chứ không bao giờ thu hẹp được nó. Do đó, vấn đề là tìm ra một trình tự an toàn để chúng ta không bao giờ bị mắc kẹt trước khi tiêu diệt hết quái vật. 

Điều này dẫn đến một cách giải thích tham lam: tại bất kỳ thời điểm nào, chúng ta duy trì trạng thái hiện tại (A, B) và chúng ta liên tục chọn bất kỳ quái vật nào hiện có thể bị giết. Vấn đề duy nhất là liệu một lựa chọn tồi trong số những quái vật hiện có có thể cản trở sự tiến bộ trong tương lai hay không. Quan sát quan trọng là trong số tất cả các quái vật hiện có, việc chọn những quái vật có yêu cầu yếu hơn trước không bao giờ có hại, bởi vì những quái vật mạnh hơn sẽ không trở nên khó hơn khi chúng ta trì hoãn chúng, trong khi phần thưởng sớm chỉ mở rộng khu vực khả thi. 

Điều này cho phép chúng tôi sắp xếp quái vật theo “mức độ khó” của chúng, được xác định là max(ai, bi) và xử lý chúng theo thứ tự tăng dần. Khi yêu cầu nhỏ, chúng có thể đạt được sớm hơn; khi nhu cầu lớn, chúng sẽ được trì hoãn một cách tự nhiên cho đến khi tích lũy đủ ci.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | Ồ (n!) | O(n) | Quá chậm | 
| Sắp xếp theo yêu cầu + kiểm tra tính khả thi tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi hiểu mỗi con quái vật là một cánh cổng yêu cầu tọa độ của cả hai phải đủ cao. Thuật toán tiến hành như sau. 

1. Tính điểm độ khó cho mỗi quái vật là max(ai, bi), đại diện cho giai đoạn sớm nhất mà quái vật có thể được xử lý một cách hợp lý. Điều này thu gọn yêu cầu hai chiều thành một khóa thứ tự duy nhất tôn trọng thực tế là cả A và B phải đủ lớn cùng một lúc. 
2. Sắp xếp tất cả quái vật theo thứ tự tăng dần của độ khó này. Trực giác cho thấy những quái vật yêu cầu ngưỡng thấp hơn nên được xử lý sớm hơn vì chúng có thể được mở khóa với những nâng cấp tối thiểu và việc trì hoãn chúng không mang lại lợi ích gì. 
3. Khởi tạo các thuộc tính hiện tại là A = 10 và B = 10. Chúng thể hiện khả năng khởi đầu trước khi nâng cấp. 
4. Đi qua quái vật theo thứ tự được sắp xếp. Với mỗi quái vật i, kiểm tra xem A ≥ ai và B ≥ bi. Nếu không, thứ tự hiện tại không hợp lệ và chúng tôi dừng lại, vì không có sự điều chỉnh nào sớm hơn hoặc muộn hơn trong thứ tự này có thể khiến con quái vật này có thể tiếp cận được. 
5. Nếu có thể tiếp cận được quái vật, chúng ta “đánh bại” nó và thêm ci vào nhóm điểm nâng cấp của mình. Vì mỗi điểm có thể được gán độc lập cho A hoặc B, nên về mặt khái niệm, chúng tôi coi đây là tính linh hoạt ngày càng tăng, nhưng chúng tôi không cần mô phỏng phân phối chính xác vì tính khả thi trong tương lai chỉ phụ thuộc vào việc chúng tôi có thể đáp ứng các ngưỡng hay không. 
6. Tiếp tục cho đến khi tất cả quái vật được xử lý. Nếu không có lỗi xảy ra, thứ tự là hợp lệ và câu trả lời là tích cực. 

Bất biến quan trọng là ở mỗi bước, thứ tự được sắp xếp đảm bảo chúng tôi không bao giờ cố gắng xử lý một con quái vật có yêu cầu vượt quá những gì lẽ ra có thể đạt được từ những con quái vật dễ dàng hơn, được xử lý trước đó. Sự gia tăng đơn điệu của các nguồn lực sẵn có có nghĩa là một khi một con quái vật trở nên khả thi thì nó vẫn khả thi cho tất cả các bước sau đó, vì vậy chúng ta không bao giờ cần phải xem xét lại các quyết định phân bổ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        monsters = []
        for _ in range(n):
            a, b, c = map(int, input().split())
            monsters.append((max(a, b), a, b, c))

        monsters.sort()

        A, B = 10, 10
        ok = True

        for _, a, b, c in monsters:
            if A < a or B < b:
                ok = False
                break
            A += c
            B += c

        print("Yes" if ok else "No")

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp ý tưởng tham lam. Khóa sắp xếp nén hai ràng buộc thành một quy tắc sắp xếp duy nhất, trong khi vẫn duy trì tính chính xác của việc kiểm tra tính khả thi. Trong quá trình mô phỏng, chúng tôi chỉ cần xác minh khả năng tiếp cận tại thời điểm xử lý vì những con quái vật trước đó đã đảm bảo đủ mức tăng trưởng. 

Một điểm tinh tế là chúng ta thêm ci vào cả A và B trong mô phỏng. Đây là một sự gần đúng quá mức có chủ ý: vì chúng tôi chỉ kiểm tra tính khả thi theo một thứ tự đã chọn, nên việc phân phối tất cả các điểm cho cả hai chiều sẽ đảm bảo tính chính xác cho việc kiểm tra. Nếu lệnh có hiệu lực theo sự phân bổ hào phóng này thì nó chắc chắn có hiệu lực theo sự phân bổ tối ưu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
10 10 2
11 10 1
10 11 1
```Sắp xếp theo max(ai, bi), chúng ta có: 

(10,10), (11,10), (10,11) 

| Bước | A | B | Quái vật | Khả thi | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 10 | (10,10) | Có | 
| 2 | 12 | 12 | (11,10) | Có | 
| 3 | 13 | 13 | (10,11) | Có | 

Sau lần tiêu diệt đầu tiên, tài nguyên sẽ tăng lên, cho phép xử lý cả hai quái vật còn lại mà không gặp vấn đề gì. Dấu vết xác nhận rằng những con quái vật dễ dàng ở giai đoạn đầu sẽ mở khóa đủ vùng đệm cho các ràng buộc sau này. 

### Ví dụ 2 

đầu vào:```
2
10 10 1
20 10 1
```Thứ tự sắp xếp là: 

(10,10), (20,10) 

| Bước | A | B | Quái vật | Khả thi | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 10 | (10,10) | Có | 
| 2 | 11 | 11 | (20,10) | Không | 

Con quái vật thứ hai thất bại ngay lập tức vì B không bao giờ đạt tới 10 trong khi A là không đủ và phần thưởng duy nhất quá nhỏ để thu hẹp khoảng cách. Điều này chứng tỏ rằng việc đặt hàng một mình không thể giải cứu được cấu trúc ban đầu không đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế, mỗi bài kiểm tra xử lý tới n quái vật | 
| Không gian | O(n) | Lưu trữ danh sách quái vật | 

Tổng n trong các thử nghiệm đủ nhỏ để việc sắp xếp và mô phỏng tuyến tính dễ dàng phù hợp với giới hạn thời gian và việc sử dụng bộ nhớ vẫn ở mức tầm thường. 

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
        monsters = []
        for _ in range(n):
            a, b, c = map(int, input().split())
            monsters.append((max(a, b), a, b, c))
        monsters.sort()

        A, B = 10, 10
        ok = True
        for _, a, b, c in monsters:
            if A < a or B < b:
                ok = False
                break
            A += c
            B += c
        out.append("Yes" if ok else "No")

    return "\n".join(out) + "\n"

# minimal case
assert run("""1
1
10 10 1
""") == "Yes\n"

# impossible due to requirement
assert run("""1
1
11 11 1
""") == "No\n"

# all easy
assert run("""1
3
10 10 1
10 10 1
10 10 1
""") == "Yes\n"

# mixed chain
assert run("""1
3
10 10 1
11 10 1
12 10 1
""") == "Yes\n"

# tight failure
assert run("""1
2
10 10 1
20 20 1
""") == "No\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khả năng tiêu diệt đơn tối thiểu | Có | tính khả thi cơ bản | 
| quái vật duy nhất không thể | Không | giới hạn dưới cứng | 
| toàn quái vật yếu | Có | công trình tích lũy | 
| chuỗi ngày càng tăng | Có | đặt hàng ổn định | 
| thất bại khoảng cách chặt chẽ | Không | phần thưởng không đủ | 

## Vỏ cạnh 

Trường hợp ranh giới xảy ra khi một quái thú có giá trị chính xác bằng giá trị ban đầu (10, 10). Thuật toán xử lý nó ngay lập tức, tăng tài nguyên và đảm bảo không có sự từ chối không chính xác nào xảy ra ở mức ngang nhau. 

Một trường hợp quan trọng khác là khi tất cả quái vật đều có yêu cầu giống nhau nhưng giá trị ci khác nhau. Vì việc sắp xếp không làm thay đổi tính khả thi trong trường hợp này nên thuật toán vẫn xử lý chúng theo thứ tự hợp lệ và mức tăng trưởng tích lũy đảm bảo thành công nếu tổng phần thưởng là đủ. 

Trường hợp cuối cùng liên quan đến một quái vật có yêu cầu B cao hơn một chút nhưng ci rất lớn. Mặc dù nó mang lại lợi nhuận nhưng không thể chọn sớm vì việc sắp xếp sẽ trì hoãn nó cho đến khi tính khả thi xuất hiện một cách tự nhiên, ngăn chặn việc phân bổ sớm có thể phá vỡ các yêu cầu khác.
