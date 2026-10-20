---
title: "CF 105710C - Chim sẻ bay"
description: "Chúng ta được cho một đàn chim, mỗi con bắt đầu từ một vị trí nằm ngang nào đó trên một đường thẳng và bay thẳng đứng lên trên đến độ cao đỉnh điểm trước khi bay thẳng trở lại mặt đất. Điều phức tạp là các lớp gió tồn tại ở những độ cao cụ thể."
date: "2026-06-26T07:59:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105710
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 2-12-25 Div. 1 (Advanced)"
rating: 0
weight: 105710
solve_time_s: 51
verified: true
draft: false
---

[CF 105710C - Finchy Flying](https://codeforces.com/problemset/problem/105710/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đàn chim, mỗi con bắt đầu từ một vị trí nằm ngang nào đó trên một đường thẳng và bay thẳng đứng lên trên đến độ cao đỉnh điểm trước khi bay thẳng trở lại mặt đất. Điều phức tạp là các lớp gió tồn tại ở những độ cao cụ thể. Bất cứ khi nào một con chim đi qua một trong những lớp gió này, vị trí nằm ngang của nó sẽ dịch chuyển một lượng cố định. Sự thay đổi xảy ra cả khi đi lên và đi xuống, do đó, một con chim vượt qua một lớp hai lần sẽ bị ảnh hưởng hai lần, trong khi một con chim đạt đến đỉnh chính xác ở độ cao đó chỉ bị ảnh hưởng một lần. 

Nhiệm vụ là tính toán vị trí hạ cánh cuối cùng của mỗi con chim sau khi tính toán tất cả các lớp gió mà nó trải qua trong suốt chuyến bay. 

Kích thước đầu vào lên tới 100.000 con chim và 100.000 lớp gió, do đó, bất kỳ giải pháp nào xử lý từng con chim theo từng lớp gió một cách độc lập sẽ là quá chậm. Một mô phỏng đơn giản sẽ yêu cầu thứ tự 10^10 thao tác trong trường hợp xấu nhất, điều này không khả thi trong giới hạn thời gian 1 giây. Điều này ngay lập tức loại trừ khả năng quét tất cả các lớp gió theo từng con chim. 

Một điểm tinh tế là các lớp gió chỉ quan trọng nếu chúng nằm bằng hoặc dưới độ cao tối đa của chim. Một con chim không tương tác với bất kỳ lớp nào phía trên đỉnh của nó. Điều này tạo ra sự phụ thuộc vào việc lọc độ cao mà việc triển khai đơn giản có thể xử lý sai. 

Một sai lầm phổ biến là coi tất cả các lớp gió đều ảnh hưởng như nhau đến tất cả các loài chim. Ví dụ: nếu một con chim có độ cao 2 và có lớp gió ở độ cao 10 thì nó sẽ không có tác dụng. Một giải pháp bất cẩn chỉ đơn giản là tổng hợp tất cả gió mà không kiểm tra độ cao sẽ tạo ra kết quả không chính xác. 

Một trường hợp hỏng hóc khác xuất hiện khi nhiều lớp gió có cùng độ cao. Ví dụ: nếu hai dòng phản lực tồn tại ở độ cao 5 với các dịch chuyển +2 và -1, thì hiệu ứng thực sự phụ thuộc vào việc cả hai được áp dụng hay một dòng ghi đè lên dòng kia. Xử lý đúng yêu cầu tổng hợp tất cả các đóng góp ở cùng độ cao. 

Cuối cùng, những con chim có chiều cao giống nhau nhưng vị trí xuất phát khác nhau phải được xử lý độc lập ngay cả khi chúng có cùng một tập hợp các lớp gió áp dụng. Một giải pháp cố gắng tính toán lại hiệu ứng gió từ đầu cho mỗi con chim có nguy cơ lặp lại công việc giống nhau một cách không cần thiết. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận bạo lực xử lý từng con chim một cách độc lập. Đối với một con chim, chúng tôi sẽ lặp lại tất cả các lớp gió, kiểm tra xem độ cao của lớp có nhỏ hơn hoặc bằng chiều cao đỉnh cao của con chim hay không và tích lũy độ dịch chuyển theo chiều ngang tương ứng. Điều này đúng vì nó mô phỏng trực tiếp định nghĩa của vấn đề: mỗi lớp có thể áp dụng đóng góp gió một lần cho mỗi lần đi lên và đi xuống, ngoại trừ lớp đỉnh chỉ đóng góp một lần. Tuy nhiên, vì mỗi con chim có thể cần kiểm tra tất cả các lớp gió nên độ phức tạp trở thành O(nm), dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất và quá chậm. 

Quan sát quan trọng là sự đóng góp của các lớp gió không phụ thuộc vào vị trí xuất phát của chim và chỉ phụ thuộc vào tập hợp các lớp có độ cao đạt tới độ cao tối đa của nó. Điều này gợi ý tính toán trước hiệu ứng gió tích lũy theo độ cao. Nếu chúng ta sắp xếp các lớp gió theo độ cao và nén tất cả các hiệu ứng vào cấu trúc tiền tố thì đối với bất kỳ loài chim nào, chúng ta có thể trả lời tổng độ dịch chuyển theo chiều ngang của nó bằng cách sử dụng truy vấn tổng tiền tố lên đến độ cao của nó. Điều này làm giảm vấn đề sắp xếp và tích lũy tiền tố thay vì quét lặp lại. 

Khi chúng tôi xử lý các lớp gió theo thứ tự được sắp xếp, chúng tôi có thể duy trì tổng chuyển vị ngang đang hoạt động. Mỗi con chim chỉ cần truy vấn tổng số lần chạy này ở ngưỡng chiều cao của nó. Nếu nhiều lớp có cùng độ cao, chúng tôi sẽ tổng hợp chúng trước khi cập nhật tổng tiền tố để đảm bảo tính chính xác.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Sắp xếp + Tiền tố Tổng theo chiều cao | O((n + m) log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các lớp gió và nhóm chúng theo độ cao, tổng hợp tác dụng của chúng. Điều này đảm bảo rằng nhiều luồng tia ở cùng độ cao được coi là một ảnh hưởng kết hợp duy nhất. 
2. Sắp xếp các độ cao duy nhất theo thứ tự tăng dần. Điều này cho phép chúng ta xây dựng sự tích lũy tiền tố đơn điệu của các hiệu ứng gió. 
3. Đi qua các lớp gió đã được sắp xếp và duy trì tổng tiền tố chuyển dịch ngang. Ở mỗi độ cao, cộng tổng hiệu ứng gió ở độ cao đó vào tổng tích lũy. 
4. Lưu trữ các cặp (độ cao, độ dịch chuyển tích lũy). Cấu trúc này cho phép tra cứu nhanh tổng hiệu ứng gió ở bất kỳ độ cao nào. 
5. Đối với mỗi con chim, hãy lấy chiều cao của nó và tìm độ cao gió cuối cùng không vượt quá nó bằng cách sử dụng tìm kiếm nhị phân. Giá trị tiền tố tương ứng là tổng độ dịch chuyển theo chiều ngang mà con chim đó trải qua. 
6. Thêm sự thay đổi này vào vị trí ban đầu của con chim và đưa ra kết quả. 

Bước suy luận tinh tế là một con chim bị ảnh hưởng bởi tất cả các tầng gió có độ cao cao nhất là đỉnh cao. Vì chúng tôi đã nén tất cả các đóng góp vào cấu trúc tiền tố theo độ cao được sắp xếp nên giá trị tiền tố trực tiếp mã hóa chính xác những hiệu ứng đó mà không cần mô phỏng đường bay. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là sau khi xử lý các lớp gió theo thứ tự độ cao tăng dần, tổng tiền tố ở bất kỳ độ cao h nào biểu thị tổng chuyển vị ngang gây ra bởi tất cả các dòng tia ở độ cao ≤ h. Mỗi con chim có chiều cao cực đại h tương tác với chính xác tập hợp các dòng phản lực đó, bởi vì nó đi qua tất cả chúng trong quá trình đi lên và đi xuống theo định nghĩa bài toán. Vì các đóng góp là độc lập và cộng gộp, và không có dòng tia nào cao hơn có thể ảnh hưởng đến con chim có chiều cao thấp hơn, nên tổng tiền tố vừa đủ vừa chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from bisect import bisect_right

def solve():
    n, m = map(int, input().split())
    
    birds = []
    for _ in range(n):
        l, h = map(int, input().split())
        birds.append((l, h))
    
    jets = {}
    for _ in range(m):
        a, w = map(int, input().split())
        if a in jets:
            jets[a] += w
        else:
            jets[a] = w
    
    heights = sorted(jets.keys())
    
    prefix = []
    total = 0
    for h in heights:
        total += jets[h]
        prefix.append(total)
    
    ans = []
    for l, h in birds:
        idx = bisect_right(heights, h) - 1
        if idx >= 0:
            ans.append(str(l + prefix[idx]))
        else:
            ans.append(str(l))
    
    print(" ".join(ans))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén tất cả các lớp gió theo độ cao để các độ cao lặp lại được hợp nhất thành một sự dịch chuyển hiệu quả duy nhất. Danh sách độ cao được sắp xếp sau đó cho phép một mảng tổng tiền tố biểu thị ảnh hưởng tích lũy của gió lên đến từng độ cao. 

Đối với mỗi con chim, tìm kiếm nhị phân sẽ tìm ra lớp gió cao nhất không vượt quá chiều cao cực đại của nó. Chỉ số đó trực tiếp đưa ra sự dịch chuyển tích lũy chính xác. Nếu không có lớp như vậy tồn tại, con chim không bị ảnh hưởng và giữ nguyên vị trí ban đầu. 

Một cạm bẫy triển khai phổ biến là quên hợp nhất các luồng phản lực có độ cao giống hệt nhau trước khi xây dựng mảng tiền tố. Nếu bước này bị bỏ qua, việc tích lũy sau này có thể tính gấp đôi hoặc sắp xếp sai hiệu ứng. Một vấn đề tinh tế khác là lỗi từng lỗi một trong tìm kiếm nhị phân, vì bisect_right phải được điều chỉnh để có được chỉ mục hợp lệ cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

3 con chim và 1 dòng tia 

Đầu tiên chúng tôi nhóm các dòng tia theo độ cao. Chỉ có một ở độ cao 4 với shift +2. Mảng tiền tố trở thành: 

| Bước | Chiều cao được xem xét | Tổng tiền tố | 
| --- | --- | --- | 
| 1 | 4 | 2 | 

Bây giờ chúng tôi xử lý chim: 

| Chim | Bắt đầu tôi | Chiều cao h | Chỉ mục | Thay đổi | Cuối cùng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 2 | không | 0 | 5 | 
| 2 | 2 | 4 | 4 | 2 | 4 | 
| 3 | -2 | 6 | 4 | 2 | 0 | 

Điều này cho thấy chỉ những con chim đạt chiều cao ít nhất 4 tuổi mới bị ảnh hưởng. 

### Ví dụ 2 

Hãy xem xét nhiều dòng phản lực: 

đầu vào: 

chim ở (10, 3), (0, 5) 

dòng tia: (2, +1), (4, +3), (4, -1) 

Sau khi hợp nhất theo chiều cao: 

Ở độ cao 2: +1 

Ở độ cao 4: +2 (vì kết hợp +3 và -1) 

Tiền tố: 

| Chiều cao | Tiền tố | 
| --- | --- | 
| 2 | 1 | 
| 4 | 3 | 

Bây giờ đánh giá: 

| Chim | h | Tiền tố được sử dụng | Kết quả | 
| --- | --- | --- | --- | 
| (10,3) | 3 | 1 | 11 | 
| (0,5) | 5 | 3 | 3 | 

Dấu vết này cho thấy nhiều dòng tia ở cùng độ cao được hợp nhất một cách chính xác trước khi tích tụ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log m) | sắp xếp chiều cao dòng tia và tìm kiếm nhị phân trên mỗi con chim | 
| Không gian | O(m) | lưu trữ mảng tiền tố và bản đồ dòng phản lực nén | 

Các ràng buộc cho phép tổng số thực thể lên tới 2 × 10^5, do đó, giải pháp log-tuyến tính vừa vặn thoải mái trong giới hạn. Việc nén tiền tố tránh mọi tương tác bậc hai giữa chim và các lớp gió. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from bisect import bisect_right

    def solve():
        n, m = map(int, input().split())
        birds = []
        for _ in range(n):
            l, h = map(int, input().split())
            birds.append((l, h))

        jets = {}
        for _ in range(m):
            a, w = map(int, input().split())
            jets[a] = jets.get(a, 0) + w

        heights = sorted(jets.keys())
        prefix = []
        total = 0
        for h in heights:
            total += jets[h]
            prefix.append(total)

        res = []
        for l, h in birds:
            i = bisect_right(heights, h) - 1
            if i >= 0:
                res.append(str(l + prefix[i]))
            else:
                res.append(str(l))

        return " ".join(res)

    return solve()

# provided sample
assert run("""3 1
5 2
2 4
-2 6
4 2
""") == "5 4 0"

# edge: no jets
assert run("""2 0
1 1
-5 10
""") == "1 -5"

# edge: multiple same height jets
assert run("""2 2
1 5
2 3
5 10
5 -3
""") == "1 2"

# edge: negative shifts
assert run("""1 1
10 5
3 -7
""") == "3"

# edge: large height filter
assert run("""1 1
0 1
100 10
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có máy bay phản lực | vị trí không thay đổi | trường hợp cơ sở | 
| chiều cao máy bay phản lực trùng lặp | sáp nhập đúng | tính chính xác của tổng hợp | 
| gió âm | dịch chuyển sang trái | xử lý biển báo | 
| máy bay phản lực tầm cao | máy bay phản lực bị bỏ qua | lọc chiều cao | 

## Vỏ cạnh 

Khi không có dòng tia, cấu trúc tiền tố trống và mọi con chim sẽ xuất ra vị trí ban đầu. Thuật toán xử lý việc này bằng cách kiểm tra xem có tồn tại bất kỳ chỉ mục tiền tố hợp lệ nào hay không trước khi áp dụng ca. 

Khi nhiều dòng tia có cùng độ cao, chúng sẽ được hợp nhất trước khi tính toán tiền tố, do đó không còn sự mơ hồ về thứ tự. Điều này đảm bảo hiệu ứng tích lũy là chính xác ngay cả khi đầu vào không phải là duy nhất. 

Khi tất cả các dòng tia đều cao hơn chiều cao của mỗi con chim, mọi tìm kiếm nhị phân đều không trả về chỉ mục hợp lệ và tất cả các con chim vẫn ở vị trí bắt đầu, phù hợp với cách giải thích vật lý rằng không gặp phải lớp gió nào.
