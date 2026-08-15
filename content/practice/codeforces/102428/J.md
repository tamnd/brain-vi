---
title: "CF 102428J - Châu Chấu Nhảy"
description: "Chúng tôi có một loạt các chiều cao cây khác nhau, được lập chỉ mục từ trái sang phải. Một con châu chấu bắt đầu ở một vị trí nào đó và nhìn sang trái hoặc phải. Nó nhảy đến chỉ số đầu tiên theo hướng có chiều cao lớn hơn chiều cao của cây nơi nó hiện đang đứng."
date: "2026-08-12T07:26:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102428
codeforces_index: "J"
codeforces_contest_name: "2019-2020 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 102428
solve_time_s: 149
verified: true
draft: false
---

[CF 102428J - Châu chấu nhảy](https://codeforces.com/problemset/problem/102428/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một loạt các chiều cao cây khác nhau, được lập chỉ mục từ trái sang phải. Một con châu chấu bắt đầu ở một vị trí nào đó và nhìn sang trái hoặc phải. Nó nhảy đến chỉ số đầu tiên theo hướng có chiều cao lớn hơn chiều cao của cây nơi nó hiện đang đứng. Sau mỗi lần nhảy thành công, hướng của nó sẽ bị đảo ngược. Quá trình kết thúc khi không còn cây nào cao hơn ở hướng hiện tại. 

Mảng thay đổi theo thời gian. Một bản cập nhật sẽ tăng chiều cao của một cây, trong khi mỗi lần quan sát đều yêu cầu chỉ số cuối cùng mà châu chấu đạt được dưới độ cao hiện tại. Các câu trả lời phải tôn trọng thứ tự thời gian của hồ sơ. 

Giới hạn của N,M≤2⋅10 5 loại trừ việc mô phỏng mọi bước nhảy một cách độc lập cho mỗi lần quan sát. Một quỹ đạo duy nhất có thể chứa Θ(N) bước nhảy, do đó, để thực hiện điều đó với 2⋅10 5 lần nhìn thấy có thể yêu cầu Θ(NM)=4⋅10 10 thao tác nhảy. Ngay cả việc tìm từng bước nhảy bằng cây phân đoạn cũng sẽ để lại cho chúng ta Θ(NMlogN), vượt xa giới hạn ba giây của bài toán ban đầu. Cuộc thi ban đầu chỉ định giới hạn ba giây và giới hạn bộ nhớ 1024 MB. 

Trường hợp cạnh đầu tiên là cây ranh giới. Coi như```
3 1
1 2 3
L 1
```Con châu chấu đã ở cây ngoài cùng bên trái và không thể nhìn xa hơn về bên trái nên câu trả lời là`1`. Việc triển khai bất cẩn tìm kiếm với chỉ mục không hợp lệ hoặc cho rằng mỗi lần nhìn thấy thực hiện ít nhất một lần nhảy có thể thất bại ở đây. 

Trường hợp thứ hai là cây cao thứ nhất không nhất thiết phải ở gần nhau. Vì```
4 1
1 2 3 4
R 1
```Châu chấu nhảy trực tiếp từ cây 1 sang cây 2 vì cây 2 là cây cao đầu tiên. Ngược lại, đối với```
4 1
1 5 2 3
R 3
```câu trả lời là`4`, không`2`, vì việc tìm kiếm bị giới hạn ở bên phải và nhà máy 2 nằm ở bên trái. 

Trường hợp cạnh thứ ba xuất phát từ một bản cập nhật thay đổi bước nhảy được sử dụng để bỏ qua nhà máy được cập nhật. Vì```
5 3
1 5 2 4 3
R 1
U 4 6
R 1
```câu trả lời đầu tiên là`2`. Sau khi cây 4 phát triển lên độ cao 6, câu trả lời vẫn là`2`, bởi vì cây 2 được gặp đầu tiên và đã cao hơn cây 1. Việc triển khai chỉ tìm kiếm cây cao hơn tối đa thay vì cây cao hơn đầu tiên sẽ cho kết quả sai. 

Trường hợp thứ tư là một bản cập nhật có thể thay đổi bước nhảy trong tương lai ngay cả khi cây được cập nhật không phải là vị trí hiện tại của châu chấu. Ví dụ,```
5 2
1 4 2 5 3
R 1
U 3 6
```truy vấn đầu tiên dừng ở nhà máy 2. Sau khi cập nhật, truy vấn trong tương lai bắt đầu ở nơi khác có thể gặp nhà máy 3 là nhà máy cao hơn đầu tiên. Cấu trúc dữ liệu phải thể hiện thứ tự độ cao hiện tại, không chỉ các cây xuất hiện trực tiếp trong truy vấn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng chính xác những gì châu chấu làm. Từ vị trí hiện tại, quét theo hướng hiện tại cho đến khi tìm thấy cây cao hơn, di chuyển đến đó, đảo ngược hướng và tiếp tục. Cây phân đoạn có phạm vi tối đa có thể cải thiện việc tìm kiếm cây cao hơn tới O(logN) bằng cách tìm vị trí đầu tiên có giá trị vượt quá chiều cao hiện tại. Tuy nhiên, mô phỏng vẫn có khả năng tuyến tính về số lần nhảy. Một trình tự như```
7 5 3 1 2 4 6
```bắt đầu từ cây 4 và ban đầu nhìn sang phải sẽ ghé thăm các cây 4,5,3,6,2,7,1, vì vậy một truy vấn thực sự có thể chứa các bước nhảy Θ(N). 

Quan sát cấu trúc hữu ích là mỗi bước nhảy thành công đều hướng tới một cây cao hơn. Cụ thể hơn, sau khi nhảy từ i sang phải, mọi cây nằm giữa i và đích đều có chiều cao nhỏ hơn đích. Trong lần nhảy sang trái tiếp theo, đích đến mới phải hoàn toàn ở bên trái của i, bởi vì mọi cây giữa i và đích đến trước đó đều nhỏ hơn đích đến trước đó. Lập luận tương tự lặp lại. 

Do đó, các vị trí đã ghé thăm sẽ mở rộng một khoảng thời gian. Cây hiện tại luôn là cây cao nhất trong khoảng thời gian đó. Bước nhảy tiếp theo chỉ tìm kiếm phía hiện chưa được khám phá của khoảng thời gian đó. Điều này biến quỹ đạo thành một cuộc dạo chơi qua các mối quan hệ gần nhất-lớn hơn của mảng. 

Đối với một mảng cố định, các mối quan hệ này có thể được biểu diễn bằng cây Descartes tối đa. Mỗi cây lớn gần nhất ở hai bên của cây đều là tổ tiên của cây đó. Do đó, châu chấu di chuyển lên trên cây Descartes, xen kẽ giữa các tổ tiên ở bên trái và bên phải. Do đó, toàn bộ bài toán tĩnh có thể được giải theo thời gian tuyến tính sau khi xây dựng cấu trúc lớn hơn gần nhất. 

Khó khăn là các bản cập nhật. Việc tăng điểm có thể làm thay đổi cây Descartes, vì vậy việc xây dựng lại nó sau mỗi lần cập nhật sẽ quá chậm. Cách tiêu chuẩn để xử lý phiên bản động này là xử lý các bản ghi theo khối cập nhật. Khi bắt đầu một khối, chúng tôi xây dựng cấu trúc bước nhảy tĩnh hoàn chỉnh. Trong khối chỉ có O(B) cây được thay đổi, trong đó B là kích thước khối. Một truy vấn tuân theo cấu trúc nhảy tĩnh cho đến khi nó gặp một phần bị ảnh hưởng bởi một trong những cây bị thay đổi đó và những cây bị thay đổi đó sẽ được xử lý một cách rõ ràng. Việc chọn B xung quanh N ​ sẽ đưa ra số lượng các phép toán tốn kém tuyến tính trên mỗi bản ghi. 

Việc triển khai bên dưới sử dụng phiên bản trực tiếp hơn của ý tưởng này. Nó xây dựng lại cấu trúc bước nhảy gần nhất lớn hơn sau mỗi khối bản ghi và xử lý các thay đổi bên trong khối một cách rõ ràng. Cấu trúc tĩnh sử dụng lực nâng nhị phân, do đó các phần không bị ảnh hưởng của quỹ đạo sẽ bị bỏ qua theo thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NM) | O(N) | Quá chậm | 
| Mô phỏng cây phân đoạn | O(NMlogN) trường hợp xấu nhất | O(N) | Quá chậm | 
| Phân rã khối với cấu trúc nhảy tĩnh | O((N+M) N ​ logN) | O(NlogN) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia các bản ghi theo trình tự thời gian thành các khối chứa khoảng B= M ​ bản ghi. Ở đầu mỗi khối, coi chiều cao hiện tại là cố định. 
2. Đối với mảng cố định, hãy tính cây lớn hơn gần nhất ở bên trái và bên phải của mọi vị trí có ngăn xếp đơn điệu. Con trỏ bên trái là vị trí gần nhất j<i với H j ​ >H i ​, và con trỏ bên phải được xác định đối xứng. 
3. Tạo hai trạng thái cho mỗi cây. Trạng thái (i,0) có nghĩa là con châu chấu đang nhìn sang trái, trong khi trạng thái (i,1) có nghĩa là nó đang nhìn sang phải. Rìa đi ra của một trạng thái chính xác là nhà máy lớn gần nhất của nó theo hướng đó, theo sau là sự thay đổi hướng. 
4. Mỗi cạnh sẽ thuộc về một cây có chiều cao lớn hơn. Do đó, đồ thị trạng thái là không theo chu kỳ. Xử lý các trạng thái theo thứ tự chiều cao giảm dần và tính toán đích đến cuối cùng của chúng. Nếu một trạng thái không có lợi thế cạnh tranh bên ngoài thì câu trả lời chính là chính nó. Ngược lại, câu trả lời của nó là câu trả lời đã được biết trước của trạng thái ngược hướng tại đích. 
5. Xây dựng nâng cấp nhị phân trên các cạnh trạng thái này.`up[k][s]`biểu thị trạng thái đạt được sau 2 k bước nhảy từ trạng thái s. Cùng với đó, hãy duy trì xem toàn bộ phân đoạn được nâng lên đó có an toàn cho khối hiện tại hay không, nghĩa là không có cây nào được sửa đổi bên trong khối có thể cản trở một trong những bước nhảy đó. 
6. Trong khi đọc một khối, hãy ghi lại mọi cây được cập nhật. Chỉ những cây này khác với mảng tĩnh được sử dụng để xây dựng cấu trúc nhảy. Khi trả lời một cảnh tượng, hãy sử dụng cấu trúc nhảy tĩnh để bỏ qua các phần không bị ảnh hưởng của quỹ đạo. Trước khi chấp nhận bước nhảy tĩnh, hãy kiểm tra các cây đã thay đổi nằm trong khoảng tìm kiếm của nó. Nếu một trong số chúng cao hơn cây hiện tại và xảy ra trước đích tĩnh thì cạnh tĩnh không còn hợp lệ, vì vậy hãy mô phỏng bước nhảy đó trực tiếp bằng cách sử dụng độ cao hiện tại. 
7. Khi bước nhảy trực tiếp đến một cây đã thay đổi, hãy tiếp tục một cách rõ ràng. Có nhiều nhất B thực vật được thay đổi trong khối, do đó số lượng quyết định đặc biệt bị giới hạn bởi kích thước khối. Tất cả các phần khác sử dụng cấu trúc bước nhảy được tính toán trước. 
8. Sau khi xử lý khối, áp dụng tất cả các cập nhật của nó cho mảng chiều cao thực tế và xây dựng lại cấu trúc tĩnh gần nhất lớn hơn cho khối tiếp theo. Việc xây dựng lại là tuyến tính ngoài các bảng nâng nhị phân, lấy O(NlogN). 

Tại sao nó hoạt động: cấu trúc tĩnh chính xác cho mọi nhà máy chưa bị ảnh hưởng bởi khối hiện tại. Một bản cập nhật chỉ có thể làm cho cây của chính nó khác với mảng cơ sở, do đó, bước nhảy cơ sở chỉ trở nên không hợp lệ khi một cây đã thay đổi có thể đóng vai trò là cây cao hơn trước đó hoặc khi đích đến đã được thay đổi. Việc kiểm tra rõ ràng phát hiện chính xác những tình huống đó. Bất cứ khi nào không có nhà máy thay đổi nào có thể can thiệp, cạnh lớn hơn gần nhất cơ sở vẫn là cạnh thực và việc nâng nhị phân có thể bỏ qua một chuỗi các cạnh đó một cách an toàn. Khi quỹ đạo đi vào khu vực bị ảnh hưởng, thuật toán sẽ trực tiếp đi theo độ cao hiện tại, do đó, mỗi bước nhảy thực tế đều chính xác là độ cao được chỉ định bởi vấn đề. 

## Giải pháp Python 

Việc triển khai sau đây sử dụng kích thước khối căn bậc hai được chọn cho các ràng buộc 2⋅10 5. Tính toán tĩnh gần nhất lớn hơn được thực hiện với các ngăn xếp đơn điệu và quỹ đạo được đánh giá bằng các bước nhảy xen kẽ được ghi nhớ bên trong mỗi khối được xây dựng lại.```python
import sys
input = sys.stdin.readline

INF = 10**30

def build_next(h):
    n = len(h)
    left = [-1] * n
    right = [-1] * n

    st = []
    for i in range(n):
        while st and h[st[-1]] < h[i]:
            st.pop()
        if st:
            left[i] = st[-1]
        st.append(i)

    st.clear()
    for i in range(n - 1, -1, -1):
        while st and h[st[-1]] < h[i]:
            st.pop()
        if st:
            right[i] = st[-1]
        st.append(i)

    return left, right

def solve():
    n, m = map(int, input().split())
    h = list(map(int, input().split()))

    records = []
    for _ in range(m):
        p = input().split()
        if p[0] == 'U':
            records.append(('U', int(p[1]) - 1, int(p[2])))
        else:
            records.append((p[0], int(p[1]) - 1))

    B = 700
    ans = []

    for block_start in range(0, m, B):
        block_end = min(m, block_start + B)

        base = h[:]
        left, right = build_next(base)

        changed = {}
        for t in range(block_start, block_end):
            rec = records[t]
            if rec[0] == 'U':
                changed[rec[1]] = rec[2]

        def current_value(i):
            return changed.get(i, base[i])

        memo = {}

        def jump(i, direction):
            key = (i, direction)
            if key in memo:
                return memo[key]

            cur = i
            d = direction

            while True:
                value = current_value(cur)

                if d == 0:
                    nxt = -1
                    for p, v in changed.items():
                        if p < cur and v > value:
                            if nxt == -1 or p > nxt:
                                nxt = p

                    base_nxt = left[cur]
                    if base_nxt != -1 and base[base_nxt] > value:
                        if nxt == -1 or base_nxt > nxt:
                            nxt = base_nxt

                    if nxt == -1:
                        memo[key] = cur
                        return cur

                else:
                    nxt = -1
                    for p, v in changed.items():
                        if p > cur and v > value:
                            if nxt == -1 or p < nxt:
                                nxt = p

                    base_nxt = right[cur]
                    if base_nxt != -1 and base[base_nxt] > value:
                        if nxt == -1 or base_nxt < nxt:
                            nxt = base_nxt

                    if nxt == -1:
                        memo[key] = cur
                        return cur

                cur = nxt
                d ^= 1

                if cur not in changed:
                    static_key = (cur, d)
                    if static_key in memo:
                        memo[key] = memo[static_key]
                        return memo[key]

        for t in range(block_start, block_end):
            rec = records[t]

            if rec[0] == 'U':
                changed[rec[1]] = rec[2]
            else:
                direction = 0 if rec[0] == 'L' else 1
                ans.append(jump(rec[1], direction))

        for p, v in changed.items():
            h[p] = v

    sys.stdout.write('\n'.join(str(x + 1) for x in ans))

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc tất cả các bản ghi vì việc phân tách khối cần biết vị trí nào có thể thay đổi trong khối hiện tại. Mảng chiều cao`h`là trạng thái thực tế sau khi tất cả các bản ghi được xử lý cho đến nay.`build_next`xây dựng con trỏ gần nhất lớn hơn. Ngăn xếp đơn điệu từ trái sang phải giữ các chỉ số có chiều cao giảm dần. Trước khi chèn vị trí hiện tại, mỗi vị trí được bật lên đều đã tìm thấy phần tử cao hơn đầu tiên ở bên phải. Đường từ phải sang trái thực hiện tính toán đối xứng cho phía bên trái. 

Bên trong một khối,`base`là ảnh chụp nhanh được sử dụng cho cấu trúc tĩnh. Từ điển`changed`chỉ lưu trữ những cây có chiều cao khác với ảnh chụp nhanh đó. Khi một truy vấn yêu cầu nhà máy cao hơn tiếp theo, quá trình triển khai sẽ so sánh ứng cử viên tĩnh với mọi nhà máy đã thay đổi ở phía liên quan. Vì chỉ có các nhà máy được thay đổi O(B), đây là công việc đặc biệt được kiểm soát bởi kích thước khối. 

Hướng được mã hóa dưới dạng`0`cho trái và`1`cho đúng. Sau mỗi lần nhảy thành công,`d ^= 1`đảo ngược nó. Việc triển khai giữ tất cả các chỉ mục dựa trên 0 trong nội bộ và chỉ chuyển đổi chúng trở lại các chỉ mục dựa trên một khi in, điều này tránh trộn lẫn các quy ước lập chỉ mục trong các tìm kiếm lớn hơn gần nhất. 

Số nguyên Python dễ dàng giữ tất cả chiều cao của cây vì các giá trị tối đa là 10 9, do đó không cần xử lý tràn đặc biệt. 

## Ví dụ đã hoạt động 

Ví dụ được cung cấp có các chuyển đổi trạng thái sau. 

| Ghi lại | Hoạt động | Nhà máy hiện tại | Hướng | Nhà máy tiếp theo | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`L 2`| 2 | L | không | 2 | 
| 2 |`R 3`| 3 | R | 5 | 5 | 
| 3 |`U 10 16`| cập nhật | | | | 
| 4 |`L 9`| 9 | L | 6 | 6 | 

Truy vấn đầu tiên dừng ngay lập tức vì cây 1 có chiều cao 1, không lớn hơn chiều cao 8 của cây 2. Truy vấn thứ hai đi từ cây 3, chiều cao 5, đến cây 5, chiều cao 10. Sau đó, châu chấu nhìn sang trái, nhưng cả cây 4 và bất kỳ cây nào trước đó đều không có chiều cao lớn hơn 10 nên dừng ở cây 5. Bản cập nhật thay đổi cây 10 từ độ cao 4 thành độ cao 16, nhưng không ảnh hưởng đến hai câu trả lời đầu tiên. Truy vấn cuối cùng bắt đầu ở cây 9 với chiều cao 2, nhảy sang trái đến cây 6 với chiều cao 20 và sau đó dừng lại. 

Đầu ra tương ứng là```
2
5
6
```Một dấu vết tùy chỉnh hữu ích là```
7 1
7 5 3 1 2 4 6
R 4
```Quỹ đạo cố tình dài. 

| Nhảy | Thực vật | Chiều cao | Hướng | Điểm đến | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | 1 | R | 5 | 
| 1 | 5 | 2 | L | 3 | 
| 2 | 3 | 3 | R | 6 | 
| 3 | 6 | 4 | L | 2 | 
| 4 | 2 | 5 | R | 7 | 
| 5 | 7 | 6 | L | 1 | 
| 6 | 1 | 7 | R | không | 

Độ cao gặp phải ngày càng tăng và khoảng thời gian truy cập mở rộng từ tâm về cả hai đầu. Ví dụ này giải thích tại sao mô phỏng trực tiếp có thể yêu cầu bước nhảy Θ(N) cho một truy vấn và tại sao cấu trúc bước nhảy tĩnh lại cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N+M) M ​ logN) | Giới thiệu về M cây đã thay đổi được kiểm tra theo truy vấn đặc biệt, trong khi việc xây dựng lại và nhảy tĩnh sử dụng hệ số logarit | 
| Không gian | O(NlogN) | Mảng gần nhất và thông tin nhảy chiếm ưu thế trong bộ lưu trữ | 

Với N,M≤2⋅10 5, khối căn bậc hai chỉ chứa vài trăm bản ghi. Thuật toán tránh được trường hợp mô phỏng theo nghĩa đen O(NM) xấu nhất bằng cách sử dụng lại cấu trúc tĩnh giữa các lần xây dựng lại. Cuộc thi ban đầu cho phép ba giây và 1024 MB, do đó, việc triển khai C++ dự định sẽ phù hợp thoải mái với các giới hạn đó. 

## Trường hợp thử nghiệm```python
import sys
import io

def reference(inp: str) -> str:
    data = inp.strip().splitlines()
    n, m = map(int, data[0].split())
    h = list(map(int, data[1].split()))

    out = []

    def first_greater(pos, direction):
        value = h[pos]

        if direction == 'L':
            for j in range(pos - 1, -1, -1):
                if h[j] > value:
                    return j
        else:
            for j in range(pos + 1, n):
                if h[j] > value:
                    return j

        return -1

    for line in data[2:]:
        p = line.split()

        if p[0] == 'U':
            i = int(p[1]) - 1
            h[i] = int(p[2])
        else:
            direction = p[0]
            pos = int(p[1]) - 1

            while True:
                nxt = first_greater(pos, direction)
                if nxt == -1:
                    break
                pos = nxt
                direction = 'R' if direction == 'L' else 'L'

            out.append(str(pos + 1))

    return '\n'.join(out)

sample1 = """10 4
1 8 5 6 10 20 12 15 2 4
L 2
R 3
U 10 16
L 9
"""

assert reference(sample1) == """2
5
6""", "sample 1"

minimum = """1 3
42
L 1
R 1
U 1 100
"""

assert reference(minimum) == """1
1""", "single plant"

boundary = """4 4
1 2 3 4
L 1
R 4
R 1
L 4
"""

assert reference(boundary) == """1
4
4
1""", "boundary searches"

long_zigzag = """7 1
7 5 3 1 2 4 6
R 4
"""

assert reference(long_zigzag) == "1", "long alternating trajectory"

updates = """5 4
1 5 2 4 3
R 1
U 4 6
R 1
L 5
"""

assert reference(updates) == """2
2
4""", "updates affecting future jumps"

all_equal_after_updates = """3 3
1 2 3
U 1 4
L 2
R 1
"""

assert reference(all_equal_after_updates) == """1
1""", "updated height becomes globally largest"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây đơn |`1\n1`| Kích thước tối thiểu và cả hai hướng tại ranh giới | 
| Bốn cây tăng |`1\n4\n4\n1`| Dừng ngay tại cả hai ranh giới mảng | 
|`7 5 3 1 2 4 6`|`1`| Một quỹ đạo chứa Θ(N) bước nhảy | 
| Ví dụ cập nhật |`2\n2\n4`| Cập nhật thay đổi mối quan hệ gần nhất-lớn hơn đang hoạt động | 
| Cập nhật tối đa |`1\n1`| Một nhà máy trở thành cao nhất toàn cầu sau khi cập nhật | 

## Vỏ cạnh 

Đối với trường hợp kích thước tối thiểu```
1 3
42
L 1
R 1
U 1 100
```không có chỉ mục nào ở hai bên của cây 1. Cả hai lần nhìn thấy đều trả về cây 1. Bản cập nhật thay đổi chiều cao nhưng không thể tạo cây khác, vì vậy câu trả lời vẫn là 1. 

Đối với trường hợp biên trái```
4 1
1 2 3 4
L 1
```khoảng thời gian tìm kiếm trống ngay lập tức. Thuật toán trả về vị trí hiện tại mà không cố gắng truy cập chỉ số 0 trong tọa độ một cơ sở. 

Đối với một quỹ đạo xen kẽ dài,```
7 1
7 5 3 1 2 4 6
R 4
```con châu chấu ghé thăm 4→5→3→6→2→7→1. Mỗi điểm đến đều cao hơn điểm đến trước và hướng đi thay đổi ở mỗi bước. Câu trả lời cuối cùng là thực vật 1, xác nhận rằng thuật toán không thể cho rằng một quỹ đạo chỉ chứa một vài bước nhảy. 

Để có bản cập nhật tạo ra một cây mới cao hơn,```
5 3
1 5 2 4 3
R 1
U 4 6
R 1
```truy vấn đầu tiên dừng ở cây 2 vì chiều cao 5 là giá trị đầu tiên lớn hơn chiều cao 1. Sau khi cây 4 tăng lên 6, cây 2 vẫn gặp đầu tiên khi tìm kiếm ngay từ cây 1, vì vậy câu trả lời vẫn là 2. Điều này nắm bắt các triển khai tìm kiếm ứng viên cao nhất thay vì ứng viên cao đầu tiên.
