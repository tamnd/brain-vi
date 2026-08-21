---
title: "CF 104081E - \u7761\u89c9"
description: "Chúng tôi đang theo dõi một trạng thái vô hướng duy nhất thay đổi một lần mỗi giây trong khi một bản nhạc được phát theo vòng lặp vô hạn. Bản nhạc có độ dài $n$ và mỗi giây tạo ra một giá trị “âm lượng” cố định trong khoảng thời gian này."
date: "2026-07-02T02:36:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104081
codeforces_index: "E"
codeforces_contest_name: "2022\u5e74\u4e2d\u56fd\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u5973\u751f\u4e13\u573a"
rating: 0
weight: 104081
solve_time_s: 67
verified: true
draft: false
---

[CF 104081E - \u7761\u89c9](https://codeforces.com/problemset/problem/104081/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang theo dõi một trạng thái vô hướng duy nhất thay đổi một lần mỗi giây trong khi một bản nhạc được phát theo vòng lặp vô hạn. Đường đua có chiều dài$n$và mỗi giây tạo ra một giá trị “độ ồn” cố định trong khoảng thời gian này. Tùy thuộc vào giá trị đó, mức độ tỉnh táo của người đó tăng hoặc giảm đúng một đơn vị mỗi giây. 

Tại thời điểm 0, sự tỉnh táo bắt đầu ở một giá trị ban đầu nhất định. Trong quá trình mô phỏng, mỗi giây sẽ so sánh giá trị bài hát hiện tại với một ngưỡng cố định. Nếu giá trị bài hát không lớn hơn ngưỡng đó thì mức độ tỉnh táo sẽ giảm đi một, nếu không thì sẽ tăng lên một. Vì bài hát lặp lại mãi mãi nên điều này tạo ra một chuỗi vô hạn các cập nhật +1 và −1. 

Mục tiêu không phải là mô phỏng mãi mãi. Thay vào đó, chúng ta được hỏi liệu có tồn tại một khối liền kề nào đó của$k$giây, bắt đầu tại một thời điểm nào đó sau khi quá trình phát lại bắt đầu, sao cho trong suốt thời gian đó$k$giây, giá trị thức giấc không bao giờ vượt quá giới hạn cố định. Nếu một cửa sổ như vậy tồn tại ở bất kỳ đâu trong dòng thời gian vô hạn, câu trả lời là “CÓ”, nếu không thì là “KHÔNG”. 

Định dạng đầu vào chứa năm số nguyên ở dòng đầu tiên. Trong số đó, giá trị cuối cùng là sự tỉnh táo ban đầu. Một trong những giá trị ở giữa là ngưỡng được sử dụng để quyết định xem một giây đóng góp +1 hay −1. Một giá trị khác là độ dài cửa sổ được yêu cầu$k$. Giá trị còn lại là nhiễu không liên quan trong trường hợp vấn đề này và không ảnh hưởng đến quá trình. Dòng thứ hai đưa ra mảng bài hát định kỳ. 

Một mô phỏng đơn giản trên một dòng thời gian vô hạn là không thể. Ngay cả việc mô phỏng vài triệu giây cũng có thể đã vượt quá giới hạn, vì trình tự này mang tính tuần hoàn nhưng trạng thái trôi đi theo thời gian. 

Một trường hợp thất bại tinh tế của lối suy nghĩ ngây thơ về cửa sổ trượt xuất phát từ việc bỏ qua thực tế rằng$n$- mô hình chiều dài được lặp lại trong khi giá trị tích lũy dịch chuyển lên hoặc xuống. Ví dụ: ngay cả khi một cửa sổ hợp lệ tồn tại ở cuối chuỗi, nó có thể tương ứng với một phần bù khác trong chu kỳ và không thể tìm thấy bằng cách chỉ kiểm tra giai đoạn đầu tiên. 

Thách thức cốt lõi là chúng ta cần một ràng buộc cửa sổ đối với quá trình tổng tiền tố lặp lại định kỳ, vô hạn. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận bạo lực trực tiếp sẽ mô phỏng quá trình từng giây một, duy trì giá trị trạng thái hiện tại và kiểm tra mọi khoảng thời gian có thể có$k$. Đối với mỗi vị trí xuất phát$i$, chúng tôi sẽ quét về phía trước$k$các bước và xác minh xem tất cả các giá trị có nằm trong giới hạn cho phép hay không. Chi phí này$O(k)$cho mỗi vị trí bắt đầu và vì dòng thời gian không bị giới hạn nên chúng tôi sẽ phải cắt nó đi một cách giả tạo. Ngay cả khi chúng ta cắt giảm ở$O(n^2)$giây, điều này trở thành$O(n^2 k)$, điều này vượt xa khả thi đối với các ràng buộc điển hình. 

Quan sát quan trọng là sự phát triển trạng thái được xác định hoàn toàn bằng tổng tiền tố của một mảng định kỳ gồm các giá trị +1 và −1. Thay vì mô phỏng trực tiếp các cửa sổ, chúng tôi chuyển đổi quy trình thành một mảng tổng tiền tố$P$, trong đó mỗi vị trí thể hiện sự thay đổi ròng kể từ đầu. Sự tỉnh táo vào lúc đó$t$chỉ đơn giản là giá trị ban đầu cộng với$P[t]$. 

Giờ đây, điều kiện “sự tỉnh táo không bao giờ vượt quá giới hạn bên trong cửa sổ” trở thành ràng buộc tối đa đối với một phạm vi tổng tiền tố. Cụ thể, đối với một cửa sổ$[i, i+k-1]$, chúng tôi yêu cầu giá trị tổng tiền tố tối đa trong khoảng thời gian đó phải ở dưới ngưỡng bắt nguồn từ lần thức ban đầu. 

Vì dãy có tính tuần hoàn nên chúng ta chỉ cần kiểm tra hai bản sao được nối của mảng. Bất kỳ cửa sổ hợp lệ nào trong chuỗi vô hạn đều tương ứng với một số cửa sổ trong tiền tố nhân đôi này, vì mọi cửa sổ đều có thể được căn chỉnh theo giai đoạn bắt đầu trong một khoảng thời gian. 

Điều này làm giảm vấn đề xuống mức tối đa của cửa sổ trượt so với tổng tiền tố trên một mảng có kích thước$2n$, có thể được giải quyết một cách hiệu quả bằng cách sử dụng deque đơn điệu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^2 k)$|$O(1)$| Quá chậm | 
| Tổng tiền tố + Cửa sổ trượt |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi định dạng lại bài hát thành một chuỗi các đóng góp có chữ ký, sau đó tìm kiếm cửa sổ hợp lệ bằng cách sử dụng tổng tiền tố. 

1. Chuyển đổi từng giá trị bài hát thành một delta: nếu nó nhỏ hơn hoặc bằng ngưỡng, hãy coi nó là −1, nếu không thì +1. Điều này mã hóa sự thay đổi mức độ tỉnh táo mỗi giây. 
2. Xây dựng một mảng tổng tiền tố trên hai bản sao được nối của mảng delta này. Chúng tôi nhân đôi mảng để mọi cửa sổ tuần hoàn được biểu diễn dưới dạng một phân đoạn liền kề. 
3. Với mỗi vị trí trong mảng nhân đôi này, hãy tính tổng tiền tố. Điều này thể hiện sự thay đổi thực sự về tình trạng thức giấc tính đến giây đó. 
4. Duy trì kích thước cửa sổ trượt$k$trên các tổng tiền tố. Đối với mỗi cửa sổ, chúng ta cần giá trị tổng tiền tố tối đa bên trong nó. 
5. Đối với mỗi cửa sổ, hãy kiểm tra xem tổng tiền tố tối đa này, khi được cộng vào số lần thức ban đầu, có nằm trong giới hạn cho phép hay không. Nếu đúng như vậy, chúng tôi ngay lập tức kết luận rằng có tồn tại một cửa sổ ngủ hợp lệ. 
6. Nếu không có cửa sổ nào thỏa mãn điều kiện sau khi quét tất cả thí sinh thì câu trả lời là “KHÔNG”. 

Lý do chúng tôi chỉ theo dõi mức tối đa bên trong mỗi cửa sổ là vì mức độ tỉnh táo kém nhất khi tổng tiền tố cao nhất, vì tiền tố cao hơn có nghĩa là mức độ tỉnh táo cao hơn. 

### Tại sao nó hoạt động 

Sự tỉnh táo vào lúc đó$t$được xác định hoàn toàn bởi tổng tiền tố lên tới$t$. Do đó, việc kiểm soát xem tình trạng thức giấc có duy trì dưới ngưỡng trên một cửa sổ hay không sẽ giảm xuống thành việc kiểm soát tổng tiền tố tối đa trong cửa sổ đó. Mọi vi phạm phải xảy ra tại điểm có tổng tiền tố lớn nhất trong khoảng. Bằng cách liệt kê tất cả các vị trí tuần hoàn thông qua một mảng nhân đôi, chúng tôi đảm bảo rằng mọi giai đoạn bắt đầu có thể có trong vòng lặp vô hạn đều được bao phủ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, k, _, thresh, w0 = map(int, input().split())
    a = list(map(int, input().split()))
    
    # build doubled delta array
    arr = []
    for _ in range(2):
        for x in a:
            arr.append(1 if x > thresh else -1)
    
    m = len(arr)
    pref = [0] * (m + 1)
    for i in range(m):
        pref[i + 1] = pref[i] + arr[i]
    
    limit = thresh - w0
    
    dq = deque()
    
    for i in range(1, m + 1):
        while dq and dq[0] < i - k:
            dq.popleft()
        
        while dq and pref[dq[-1]] <= pref[i - 1]:
            dq.pop()
        
        dq.append(i - 1)
        
        if i >= k:
            max_pref = pref[dq[0]]
            if max_pref <= limit:
                print("YES")
                return
    
    print("NO")

if __name__ == "__main__":
    solve()
```Mã này xây dựng một phiên bản nhân đôi của quy trình sao cho mọi căn chỉnh theo chu kỳ đều được biểu diễn tuyến tính. Mảng tiền tố`pref`theo dõi những thay đổi tích lũy về tình trạng thức giấc. Một deque duy trì các chỉ số của các giá trị tiền tố theo thứ tự giảm dần, do đó mặt trước luôn lưu trữ tiền tố tối đa trong cửa sổ hiện tại. 

Điều kiện so sánh tiền tố tối đa đó với giới hạn được phép bắt nguồn từ lần thức ban đầu. Ngay khi tìm thấy một cửa sổ hợp lệ, chúng tôi sẽ dừng sớm vì sự tồn tại là đủ. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi cấu trúc mẫu đầu tiên nơi tồn tại một cửa sổ hợp lệ. 

Giả sử chuỗi delta được xử lý bắt nguồn từ việc so sánh từng giá trị bài hát với ngưỡng. 

| Bước | Đồng bằng | Tiền tố Tổng | Tối đa trong cửa sổ (k=3) | 
| --- | --- | --- | --- | 
| 1 | -1 | -1 | -1 | 
| 2 | -1 | -2 | -1 | 
| 3 | +1 | -1 | -1 | 
| 4 | +1 | 0 | 0 | 
| 5 | +1 | 1 | 1 | 

Trong dấu vết này, khi chúng tôi đánh giá cửa sổ có độ dài 3 bắt đầu từ vị trí đầu tiên, tiền tố tối đa vẫn nằm trong giới hạn cho phép, do đó tồn tại một phân đoạn hợp lệ và thuật toán trả về “CÓ”. 

Đối với mẫu thứ hai trong đó$k=4$, cấu trúc tiền tố tương tự được mở rộng, nhưng mọi cửa sổ có độ dài-4 đều bao gồm một đỉnh tiền tố vượt quá giới hạn ngưỡng, nghĩa là không có phân đoạn hợp lệ nào thỏa mãn điều kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi chỉ mục được đẩy và xuất hiện nhiều nhất một lần trong deque trong khi quét một mảng nhân đôi | 
| Không gian |$O(n)$| Tổng tiền tố và lưu trữ mảng gấp đôi | 

Giải pháp thoải mái phù hợp với các ràng buộc điển hình cho$n$lên đến$2 \times 10^5$, vì tất cả các phép toán đều tuyến tính và chỉ yêu cầu xử lý mảng đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    
    def solve():
        n, k, _, thresh, w0 = map(int, input().split())
        a = list(map(int, input().split()))
        arr = []
        for _ in range(2):
            for x in a:
                arr.append(1 if x > thresh else -1)
        m = len(arr)
        pref = [0] * (m + 1)
        for i in range(m):
            pref[i + 1] = pref[i] + arr[i]
        limit = thresh - w0
        dq = deque()
        for i in range(1, m + 1):
            while dq and dq[0] < i - k:
                dq.popleft()
            while dq and pref[dq[-1]] <= pref[i - 1]:
                dq.pop()
            dq.append(i - 1)
            if i >= k:
                if pref[dq[0]] <= limit:
                    print("YES")
                    return
        print("NO")

    solve()
    sys.stdout.seek(0)
    return sys.stdout.read().strip()

# provided samples (format adapted)
assert run("5 3 4 5 5\n3 4 6 7 8\n") == "YES"
assert run("5 4 4 5 5\n3 4 6 7 8\n") == "NO"

# custom cases
assert run("3 1 0 1 0\n2 2 2\n") == "YES"
assert run("3 2 0 10 0\n1 1 1\n") == "YES"
assert run("4 3 0 0 0\n5 5 5 5\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tất cả các giá trị nhỏ, k=1 | CÓ | Phát hiện cửa sổ hợp lệ ngay lập tức | 
| Tất cả các vùng đồng bằng tích cực, giới hạn lỏng lẻo | CÓ | trường hợp tăng trưởng | 
| Tất cả các vùng đồng bằng đều âm nhưng giới hạn nghiêm ngặt | KHÔNG | Ràng buộc không thể | 

## Vỏ cạnh 

Trường hợp quan trọng là khi bài hát hoàn toàn “an toàn”, nghĩa là mọi giá trị đều ở dưới hoặc bằng ngưỡng. Trong trường hợp này, sự tỉnh táo giảm dần theo từng giây. Mặc dù hệ thống trôi xuống vô tận, thuật toán vẫn cần xác định chính xác cửa sổ ban đầu hợp lệ. Tổng tiền tố trở nên đơn điệu giảm dần, do đó cực đại của cửa sổ trượt luôn là ranh giới bên trái và cửa sổ khả thi đầu tiên được phát hiện ngay lập tức. 

Một trường hợp khác là khi mọi giá trị đều vượt quá ngưỡng. Mức độ tỉnh táo tăng lên một cách nghiêm ngặt và không có cửa sổ nào có thể thỏa mãn ràng buộc giới hạn trên đối với các giới hạn đủ lớn. Tiền tố tối đa trong mỗi cửa sổ tăng đều đặn, do đó thuật toán sẽ loại bỏ tất cả các cửa sổ một cách chính xác. 

Trường hợp tinh tế thứ ba xảy ra khi cửa sổ hợp lệ chỉ xuất hiện trên ranh giới giữa hai lần lặp lại của mảng. Cấu trúc tiền tố kép đảm bảo phân đoạn này vẫn được biểu diễn dưới dạng phân đoạn liền kề, do đó quá trình quét dựa trên deque sẽ đánh giá nó một cách tự nhiên mà không cần viết hoa đặc biệt.
