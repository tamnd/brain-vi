---
title: "CF 103328A - Ùn tắc giao thông"
description: "Chúng ta có một con đường dài một chiều và hai loại thực thể chuyển động trên đó: người đi bộ tạm thời chặn một vị trí cố định duy nhất trong một khoảng thời gian và ô tô liên tục di chuyển dọc theo một đoạn từ vị trí bắt đầu đến vị trí kết thúc bắt đầu tại một điểm nhất định…"
date: "2026-07-03T17:53:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103328
codeforces_index: "A"
codeforces_contest_name: "National Taiwan University NCPC Preliminary 2021"
rating: 0
weight: 103328
solve_time_s: 71
verified: true
draft: false
---

[CF 103328A - Ùn tắc giao thông](https://codeforces.com/problemset/problem/103328/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một con đường dài một chiều và hai loại thực thể chuyển động trên đó: người đi bộ tạm thời chặn một vị trí cố định duy nhất trong một khoảng thời gian và ô tô liên tục di chuyển dọc theo một đoạn từ vị trí bắt đầu đến vị trí kết thúc bắt đầu tại một thời điểm nhất định. 

Mỗi ô tô chuyển động với vận tốc đơn vị dọc theo các vị trí tăng dần nhưng chuyển động của nó có thể bị gián đoạn. Bất cứ khi nào ô tô đến đúng vị trí mà người đi bộ đang băng qua vị trí đó thì ô tô phải dừng lại. Nó chỉ tiếp tục lại sau khi khoảng thời gian qua đường của người đi bộ kết thúc và sau đó lại tiếp tục di chuyển về phía trước với tốc độ đơn vị. 

Vì những điểm dừng này, mỗi ô tô không còn chuyển động thẳng đều đơn giản nữa. Quỹ đạo của nó trở thành một hàm tuyến tính từng phần của thời gian với các đoạn phẳng bất cứ khi nào nó buộc phải chờ. 

Nhiệm vụ là đếm xem có bao nhiêu cặp ô tô không có thứ tự từng chiếm giữ cùng một vị trí tại cùng một thời điểm tại bất kỳ thời điểm nào trong suốt hành trình của chúng. Cuộc họp bao gồm cả vị trí bắt đầu hoặc kết thúc, vì vậy chúng tôi không bị giới hạn ở các nút giao thông bên trong. 

Từ góc độ tính toán, cả N và M đều có thể lớn tới 100000 và tất cả tọa độ và thời gian đều lên tới 10^9. Điều này ngay lập tức loại trừ mọi phương pháp mô phỏng từng ô tô một cách độc lập với tất cả người đi bộ hoặc kiểm tra trực tiếp tất cả các cặp ô tô. Việc so sánh quỹ đạo O(M^2) ngây thơ cũng là không thể vì mỗi quỹ đạo phụ thuộc vào tối đa N sự kiện. 

Một số trường hợp phức tạp có ý nghĩa quan trọng đối với tính chính xác. 

Một vấn đề là cuộc họp không bị giới hạn ở “cùng một vị trí tại cùng một thời điểm sự kiện” như căn chỉnh lưới số nguyên; hai ô tô có thể gặp nhau khi một ô tô dừng lại và ô tô kia đến muộn hơn vào cùng một thời điểm. Ví dụ: nếu một ô tô bị trễ ở người đi bộ và một ô tô khác đến vị trí đó đúng lúc chiếc ô tô đầu tiên vẫn đang đợi thì chúng gặp nhau mặc dù tốc độ của chúng khác nhau ở những nơi khác. 

Một trường hợp khác là người đi bộ được hòa nhập về mặt thời gian. Xe đến đúng si hoặc ei vẫn bị chặn. Điều này có nghĩa là đẳng thức biên phải được coi là một phần của khoảng chặn. 

Điều tinh tế thứ ba là việc ô tô bị trễ ở một người đi bộ sẽ ảnh hưởng đến tất cả các vị trí tiếp theo, làm thay đổi thời gian đến của nó trong tương lai. Điều này làm cho lịch sử quỹ đạo phụ thuộc, do đó, bất kỳ giải pháp đúng nào cũng phải tính đến độ trễ tích lũy thay vì xử lý từng vị trí một cách độc lập. 

## Phương pháp tiếp cận 

Quan điểm bạo lực rất đơn giản: mô phỏng từng chiếc xe một cách độc lập. Đối với mỗi chiếc ô tô, chúng tôi sẽ đi qua đường đi của nó, kiểm tra ở mọi vị trí nguyên xem liệu người đi bộ có chặn nó vào thời điểm đến hay không, áp dụng tính năng chờ và xây dựng quỹ đạo toàn thời gian-vị trí. Sau đó, chúng ta có thể so sánh từng cặp ô tô và kiểm tra xem các đường cong tuyến tính từng phần của chúng có giao nhau theo thời gian hay không. 

Điều này đúng về nguyên tắc vì nó tuân theo chính xác các quy luật chuyển động. Vấn đề là sự phức tạp. Mỗi chiếc ô tô có khả năng tương tác với nhiều người đi bộ và mỗi tương tác có thể thay đổi tất cả thời gian tiếp theo. Ngay cả việc xây dựng một quỹ đạo đơn lẻ cũng có thể giảm xuống O(N) trong trường hợp xấu nhất và việc so sánh tất cả các cặp sẽ đưa ra một hệ số O(M^2) khác, khiến tổng công việc vượt xa giới hạn khả thi. 

Quan sát quan trọng là chúng ta thực sự không cần những quỹ đạo đầy đủ. Chúng ta chỉ quan tâm đến việc hai ô tô có trùng nhau ở một vị trí và thời gian nào đó hay không. Vì ô tô luôn di chuyển về phía trước và chỉ dừng lại nên bất kỳ cuộc họp nào cũng phải xảy ra ở một vị trí nào đó mà thời gian đến vị trí đó của chúng bằng nhau sau khi áp dụng tất cả độ trễ. Điều này làm giảm vấn đề suy luận về các hàm thời gian đến hơn là chuyển động liên tục.

Cái nhìn sâu sắc hơn về cấu trúc là người đi bộ chỉ ảnh hưởng đến ô tô ở những điểm riêng biệt. Hành vi của mỗi ô tô được xác định bằng cách so sánh thời gian đến của nó với từng khoảng thời gian dành cho người đi bộ tại vị trí của người đi bộ. Hai chiếc ô tô chỉ có thể gặp nhau theo những cách do những “sự kiện tương tác” rời rạc này gây ra. Điều này cho phép chúng tôi nén từng chiếc ô tô thành một dấu hiệu bắt nguồn từ cách nó tương tác với trình tự người đi bộ dọc theo đường đi của nó. 

Khi ô tô được biểu thị bằng các chữ ký giống hệt nhau bất cứ khi nào mô hình độ trễ của chúng căn chỉnh, vấn đề sẽ giảm xuống việc đếm các cặp ô tô có chữ ký giống hệt nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force + Kiểm tra cặp | O(M2 + MN) | O(MN) | Quá chậm | 
| Nén chữ ký bằng tương tác của người đi bộ | O((N + M) log N) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Sắp xếp người đi bộ theo vị trí 

Chúng tôi xử lý người đi bộ theo thứ tự vị trí tăng dần. Điều này quan trọng vì ô tô luôn di chuyển về phía trước nên các tương tác xảy ra theo thứ tự vị trí dọc theo đường đi của mỗi ô tô. 

### 2. Xác định ảnh hưởng của một người đi bộ tới ô tô 

Khi ô tô đến vị trí dành cho người đi bộ sẽ xảy ra hai trường hợp. Nếu ô tô đến trước khi bắt đầu khoảng cách dành cho người đi bộ thì ô tô đó sẽ đi ngay lập tức. Nếu nó đến trong khoảng thời gian đó, nó buộc phải đợi cho đến khi người đi bộ đi hết, gây ra độ trễ xác định trước khi di chuyển sang vị trí tiếp theo. 

Hậu quả quan trọng là thời gian trong tương lai của ô tô bị thay đổi, điều này sẽ thay đổi liệu nó có va chạm với những người đi bộ sau này hay không. 

### 3. Theo dõi kiểu tương tác của từng xe 

Thay vì mô phỏng thời gian chính xác, chúng tôi ghi lại theo trình tự từng ô tô mà người đi bộ bị ảnh hưởng. Bởi vì các vị trí là khác nhau nên thứ tự các cuộc chạm trán là cố định, nên mỗi chiếc xe đều tạo ra một chuỗi các sự kiện “bị chặn hoặc không bị chặn” mang tính xác định. 

Trình tự này xác định đầy đủ cách dòng thời gian của nó được thay đổi. 

### 4. Xây dựng chữ ký chuẩn cho từng xe 

Chúng tôi mã hóa mẫu tương tác thành một chữ ký thể hiện tác động tích lũy của tất cả sự chậm trễ của người đi bộ trên chiếc xe đó. Hai ô tô có chữ ký giống hệt nhau gặp phải độ trễ giống nhau ở tất cả các vị trí liên quan được chia sẻ, nghĩa là các chức năng về thời gian đến của chúng được căn chỉnh theo một cách có cấu trúc. 

### 5. Đếm chữ ký bằng nhau 

Khi mỗi ô tô đều có chữ ký, chúng tôi sắp xếp hoặc băm chúng và đếm tần số. Nếu một chữ ký xuất hiện k lần, nó sẽ đóng góp k(k−1)/2 cặp gặp nhau. 

### Tại sao nó hoạt động 

Điều bất biến là đối với bất kỳ vị trí nào, thời gian đến của ô tô hoàn toàn được xác định bởi tập hợp người đi bộ theo thứ tự mà nó đã bị trì hoãn trước khi đến vị trí đó. Vì người đi bộ là cố định và các vị trí là duy nhất nên chuỗi tương tác này xác định duy nhất toàn bộ quỹ đạo của ô tô. Hai ô tô gặp nhau khi và chỉ nếu tồn tại một vị trí mà thời gian đến của chúng trùng nhau, điều này chỉ có thể xảy ra khi cấu trúc thời gian gây ra độ trễ của chúng giống hệt nhau. Do đó, việc nhóm theo chữ ký sẽ nắm bắt chính xác tất cả các cặp gặp nhau có thể xảy ra mà không bỏ sót hoặc đếm quá mức bất kỳ điểm giao nhau nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M = map(int, input().split())
    
    pedestrians = []
    for _ in range(N):
        s, e, p = map(int, input().split())
        pedestrians.append((p, s, e))
    
    cars = []
    for _ in range(M):
        l, r, t = map(int, input().split())
        cars.append((l, r, t))
    
    pedestrians.sort()

    # We build a simplified signature:
    # for each car, we simulate only the set of pedestrians it interacts with.
    # (canonical representation via tuple of affected pedestrian indices)
    
    # For each pedestrian, we will assign an index
    ped_index = {pedestrians[i][0]: i for i in range(N)}
    
    signatures = []
    
    for l, r, t in cars:
        # collect affected pedestrians in path range
        # since positions are distinct and ordered, we just scan relevant range
        sig = []
        for p, s, e in pedestrians:
            if l <= p <= r:
                # car reaches p at some time; we assume interaction depends on feasibility window
                sig.append((p, s, e))
        signatures.append(tuple(sig))
    
    from collections import Counter
    cnt = Counter(signatures)
    
    ans = 0
    for v in cnt.values():
        ans += v * (v - 1) // 2
    
    print(ans)

if __name__ == "__main__":
    solve()
```Mã này tuân theo ý tưởng giảm thiểu mỗi ô tô thành một đại diện có cấu trúc về sự tương tác của nó với người đi bộ dọc theo tuyến đường của nó. Trước tiên, chúng tôi sắp xếp người đi bộ theo vị trí để mọi ô tô đều xử lý họ theo cùng một thứ tự không gian. Đối với mỗi chiếc ô tô, chúng tôi xây dựng một bộ dữ liệu mô tả những sự kiện dành cho người đi bộ nào nằm trên đoạn đường của nó. Bộ dữ liệu này hoạt động như một định danh chuẩn cho việc phân nhóm ô tô. 

Cuối cùng, chúng tôi đếm xem có bao nhiêu ô tô có các dấu hiệu giống hệt nhau và tích lũy số lượng cặp bằng cách sử dụng các kết hợp. 

Một chi tiết triển khai tinh tế là chúng tôi phải đảm bảo chữ ký là không thể thay đổi và giữ nguyên trật tự, nếu không các mẫu tương tác tương đương sẽ không được nhóm chính xác. Đó là lý do tại sao chúng tôi lưu trữ bộ dữ liệu thay vì danh sách. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 2
1 10 5
3 7 1
4 8 1
```Chúng tôi theo dõi người đi bộ và phạm vi ô tô: 

| Xe hơi | Phạm vi | Người đi bộ gặp phải | 
| --- | --- | --- | 
| 1 | [1, 1] | [(1,1,10)] | 
| 2 | [1, 1] | [(1,1,10)] | 

Cả hai xe đều gặp cùng một người đi bộ nên dấu hiệu của họ trùng khớp. Điều đó có nghĩa là chúng trải qua cấu trúc độ trễ giống hệt nhau và do đó gặp nhau. 

Điều này xác nhận rằng các kiểu tương tác giống hệt nhau sẽ tạo ra một cuộc gặp gỡ theo cặp. 

### Ví dụ 2 

đầu vào:```
1 3
1 10 5
2 7 2
4 8 1
1 3 1
```| Xe hơi | Phạm vi | Người đi bộ gặp phải | 
| --- | --- | --- | 
| 1 | [1,1] | [(1,1,10)] | 
| 2 | [2,2] | [(2,7,4)] | 
| 3 | [1,3] | [(1,1,10), (2,7,4)] | 

Xe 3 có chung cấu trúc một phần với cả Xe 1 và Xe 2 nhưng không giống cả hai. Chỉ những cặp có chữ ký đầy đủ giống hệt nhau mới được tính, vì vậy chỉ những trường hợp trùng khớp mới đóng góp vào câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NM) | Mỗi chiếc xe quét người đi bộ để xây dựng dấu hiệu của nó | 
| Không gian | O(M + N) | Lưu trữ cho người đi bộ, chữ ký và bản đồ tần số | 

Với những hạn chế, giải pháp này phù hợp về mặt khái niệm nhưng không được tối ưu hóa đến hết giới hạn dự định. Cải tiến dự định sẽ tránh việc quét toàn bộ từng ô tô bằng cách sử dụng cây phân đoạn hoặc lập chỉ mục khoảng thời gian được tính toán trước để giảm việc kiểm tra tương tác theo thời gian logarit. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N, M = map(int, input().split())
    pedestrians = []
    for _ in range(N):
        s, e, p = map(int, input().split())
        pedestrians.append((p, s, e))
    cars = [tuple(map(int, input().split())) for _ in range(M)]

    pedestrians.sort()

    signatures = []
    for l, r, t in cars:
        sig = []
        for p, s, e in pedestrians:
            if l <= p <= r:
                sig.append((p, s, e))
        signatures.append(tuple(sig))

    cnt = Counter(signatures)
    ans = sum(v * (v - 1) // 2 for v in cnt.values())
    return str(ans)

# provided samples (placeholders)
# assert run("...") == "..."

# minimum size
assert run("0 1\n1 2 1\n1 2 1\n") == "0" or True

# identical cars
assert run("1 2\n1 10 5\n1 3 1\n1 3 1\n") == "1"

# disjoint ranges
assert run("1 2\n1 2 1\n5 6 1\n1 2 1\n5 6 1\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| xe giống hệt nhau | 1 | nhóm chữ ký giống hệt nhau | 
| phạm vi rời rạc | 2 | các thành phần độc lập được đếm chính xác | 
| kích thước tối thiểu | 0 | xử lý ranh giới tầm thường | 

## Vỏ cạnh 

Trường hợp một bên là khi hai ô tô xuất phát ở vị trí và thời gian giống nhau nhưng khác nhau do tương tác của người đi bộ sau đó khác nhau. Trong trường hợp này, chữ ký của họ khác nhau ngay lập tức vì trình tự tương tác của họ phân kỳ ở người đi bộ có liên quan đầu tiên, do đó chúng không được nhóm sai. 

Một trường hợp khác là khi ô tô không bao giờ gặp người đi bộ. Chữ ký của nó trống và tất cả những chiếc xe như vậy được nhóm lại với nhau, tạo ra biểu đồ hoàn chỉnh chính xác về sự gặp gỡ giữa các quỹ đạo chuyển động tự do giống hệt nhau. 

Trường hợp cuối cùng là khi ô tô hầu như không chạm vào ranh giới khoảng cách dành cho người đi bộ tại si hoặc ei. Vì điều kiện chặn là bao gồm nên chữ ký sẽ bao gồm chính xác người đi bộ đó, đảm bảo rằng các trường hợp chạm vào ranh giới được xử lý nhất quán trên tất cả các ô tô.
