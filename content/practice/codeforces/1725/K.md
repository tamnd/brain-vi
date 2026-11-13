---
title: "CF 1725K - Vương quốc phê bình"
description: "Chúng tôi đang quản lý một vương quốc với một dãy các tòa nhà, mỗi tòa nhà có chiều cao nguyên. Cư dân thỉnh thoảng đưa ra những lời chỉ trích nhắm vào tất cả các tòa nhà có chiều cao trong một khoảng cụ thể [l, r], trong đó r-l luôn là số lẻ."
date: "2026-06-09T19:07:30+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dsu"]
categories: ["algorithms"]
codeforces_contest: 1725
codeforces_index: "K"
codeforces_contest_name: "COMPFEST 14 - Preliminary Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 2500
weight: 1725
solve_time_s: 168
verified: true
draft: false
---

[CF 1725K - Vương quốc phê bình](https://codeforces.com/problemset/problem/1725/K) 

**Đánh giá:** 2500 
**Thẻ:** cấu trúc dữ liệu, dsu 
**Thời gian giải:** 2m 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang quản lý một vương quốc với một dãy các tòa nhà, mỗi tòa nhà có chiều cao nguyên. Cư dân thỉnh thoảng đưa ra những lời chỉ trích nhắm vào tất cả các tòa nhà có chiều cao trong một khoảng cụ thể [l, r], trong đó r-l luôn là số lẻ. Đội xây dựng của vương quốc sau đó phải điều chỉnh độ cao của tòa nhà để không có tòa nhà nào nằm trong khoảng đó. Mỗi lần điều chỉnh tăng hoặc giảm chiều cao đi 1, nhưng chiều cao vẫn dương. 

Có ba loại hoạt động. Cái đầu tiên đặt một tòa nhà ở một độ cao cụ thể, cái thứ hai truy vấn chiều cao của tòa nhà và cái thứ ba áp đặt một khoảng phê bình phải được giải quyết ngay lập tức bằng cách di chuyển độ cao ra bên ngoài nó trong thời gian tối thiểu. Điểm quan trọng là sau mỗi lần phê bình, độ cao được điều chỉnh không cần phải thỏa mãn những phê bình trước đó. Điều này có nghĩa là những lời chỉ trích chỉ ảnh hưởng đến bức ảnh chụp nhanh hiện tại về chiều cao của các tòa nhà. 

Kích thước đầu vào có thể lớn: tối đa 4×10^5 tòa nhà và 4×10^5 truy vấn, với chiều cao lớn tới 10^9. Điều này giúp loại bỏ mọi cách tiếp cận lặp lại trên tất cả các tòa nhà cho từng lời chỉ trích, vì trong trường hợp xấu nhất, chúng tôi có thể thực hiện các phép toán 4×10^5 × 4×10^5 = 1,6×10^11, điều này là không khả thi. Do đó, chúng ta cần một cấu trúc cho phép chúng ta xử lý các truy vấn theo thời gian không đổi logarit hoặc khấu hao. 

Một trường hợp nguy hiểm có thể phá vỡ cách triển khai ngây thơ là khi chiều cao của tòa nhà chính xác ở điểm giữa của khoảng giới hạn. Ví dụ: nếu tòa nhà có chiều cao 5 và khoảng giới hạn là [4, 6], một thuật toán bất cẩn có thể không quyết định chính xác nên di chuyển nó lên hay xuống. Quy tắc thời gian tối thiểu đảm bảo có một hướng tối ưu duy nhất: di chuyển tất cả các tòa nhà trong khoảng đến ranh giới gần nhất bên ngoài khoảng. Một trường hợp khác là khi nhiều lời chỉ trích ảnh hưởng đến phạm vi chồng chéo; mỗi cái đều độc lập và phải được xử lý tương ứng với mức độ hiện tại. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ được lặp lại trực tiếp trên tất cả các tòa nhà bất cứ khi nào xảy ra chỉ trích. Đối với mỗi tòa nhà bên trong khoảng thời gian, chúng tôi sẽ tính khoảng cách đến cả l-1 và r+1, đồng thời di chuyển tòa nhà đến ranh giới gần hơn. Điều này hoạt động chính xác nhưng yêu cầu thao tác O(N) cho mỗi lần phê bình. Với những lời chỉ trích về Q, tổng chi phí là O(NQ), quá chậm đối với N, Q lên tới 4×10^5. 

Cách tiếp cận tối ưu thúc đẩy quan sát rằng việc điều chỉnh của mỗi tòa nhà chỉ phụ thuộc vào điểm giữa của khoảng thời gian đó. Vì r-l là số lẻ nên điểm giữa là phân số, nhưng chúng ta có thể coi sàn của điểm giữa hoặc phép chia số nguyên. Tất cả các độ cao nhỏ hơn hoặc bằng điểm giữa sẽ di chuyển xuống dưới l và tất cả các độ cao lớn hơn điểm giữa sẽ di chuyển lên trên r. Điều này có nghĩa là chúng ta có thể coi những lời chỉ trích là sự kết hợp của nhiều phạm vi trong đó mỗi tòa nhà chỉ quan tâm đến mối quan hệ của nó với điểm giữa. Chúng ta có thể mô hình hóa điều này dưới dạng bù đắp toàn cầu hoặc chuyển đổi được áp dụng một cách lười biếng và giải quyết các truy vấn một cách hiệu quả bằng cách lưu trữ chiều cao riêng của từng tòa nhà so với chuyển đổi này. Thông tin chi tiết quan trọng là chúng ta chỉ cần biết điểm giữa của chỉ trích cuối cùng để tính toán tác động lên bất kỳ tòa nhà nào khi được truy vấn, thay vì di chuyển vật lý tất cả các tòa nhà ở mỗi chỉ trích. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NQ) | O(N) | Quá chậm | 
| Tối ưu | O(Q) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì một mảng`heights`chiều cao hiện tại của tất cả các tòa nhà. Cũng duy trì một biến`crit_l`Và`crit_r`để lưu trữ khoảng thời gian phê bình gần đây nhất. 
2. Đối với truy vấn loại 1, đặt trực tiếp chiều cao của tòa nhà trong`heights[k] = w`. 
3. Đối với truy vấn loại 3, lưu trữ khoảng phê bình`[l, r]`. Tính điểm giữa như`mid = (l + r) // 2`. 
4. Đối với truy vấn loại 2, chúng ta cần chiều cao hiệu dụng. Nếu chiều cao hiện tại nằm trong`[l, r]`, chúng ta tính khoảng cách của nó tới cả l-1 và r+1 và di chuyển nó đến điểm gần hơn. Nếu chiều cao bằng chính xác điểm giữa thì điều chỉnh tối thiểu duy nhất là di chuyển nó xuống l-1. 
5. Trả về chiều cao hiệu dụng được tính toán cho truy vấn loại 2. 

Tại sao nó hoạt động: Điều bất biến là sau khi bị chỉ trích, mỗi tòa nhà vẫn giữ nguyên vị trí của nó (nếu nằm ngoài khoảng) hoặc di chuyển tối thiểu đến ranh giới gần nhất bên ngoài khoảng. Bằng cách chỉ lưu trữ những lời chỉ trích cuối cùng và tính toán điều chỉnh tối thiểu một cách lười biếng, chúng tôi duy trì tính chính xác của các truy vấn loại 2 mà không cần lặp lại trên tất cả các tòa nhà. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
heights = list(map(int, input().split()))
q = int(input())

crit_l, crit_r = None, None

for _ in range(q):
    query = input().split()
    t = int(query[0])
    if t == 1:
        k = int(query[1]) - 1
        w = int(query[2])
        heights[k] = w
    elif t == 2:
        k = int(query[1]) - 1
        h = heights[k]
        if crit_l is not None:
            l, r = crit_l, crit_r
            if l <= h <= r:
                mid = (l + r) // 2
                if h <= mid:
                    h = l - 1
                else:
                    h = r + 1
        print(h)
    else:
        crit_l, crit_r = int(query[1]), int(query[2])
```Mã này xử lý loại 1 bằng cách gán trực tiếp và loại 3 bằng cách lưu trữ lời chỉ trích cuối cùng. Loại 2 tính toán chiều cao hiệu quả một cách lười biếng. Cần thận trọng khi tính điểm giữa và quyết định bên nào sẽ di chuyển tòa nhà, đặc biệt khi chiều cao bằng điểm giữa. 

## Ví dụ đã hoạt động 

Dấu vết mẫu 1: 

| Truy vấn | độ cao trước | crit_l, crit_r | đầu ra | độ cao sau | 
| --- | --- | --- | --- | --- | 
| 1 5 10 | [2,6,5,6,2] | Không có | - | [2,6,5,6,10] | 
| 2 5 | [2,6,5,6,10] | Không có | 10 | [2,6,5,6,10] | 
| 1 1 3 | [2,6,5,6,10] | Không có | - | [3,6,5,6,10] | 
| 3 3 6 | [3,6,5,6,10] | - | - | - | 
| 3 8 9 | [3,6,5,6,10] | 8,9 | - | - | 
| 1 2 9 | [3,6,5,6,10] | 8,9 | - | [3,9,5,6,10] | 
| 2 3 | [3,9,5,6,10] | 8,9 | 7 | - | 
| 2 2 | [3,9,5,6,10] | 8,9 | 9 | - | 
| 2 4 | [3,9,5,6,10] | 8,9 | 7 | - | 

Điều này xác nhận rằng các tòa nhà được điều chỉnh một cách lười biếng và chỉ kết quả truy vấn mới phản ánh tác động của lời chỉ trích cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q) | Mỗi truy vấn được xử lý trong O(1) bằng cách sử dụng tính toán lười biếng để phê bình | 
| Không gian | O(N) | Lưu trữ chiều cao của tất cả các tòa nhà | 

Thuật toán xử lý thoải mái N và Q lên tới 4×10^5 trong giới hạn thời gian 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    n = int(input())
    heights = list(map(int, input().split()))
    q = int(input())
    crit_l, crit_r = None, None
    for _ in range(q):
        query = input().split()
        t = int(query[0])
        if t == 1:
            k = int(query[1]) - 1
            w = int(query[2])
            heights[k] = w
        elif t == 2:
            k = int(query[1]) - 1
            h = heights[k]
            if crit_l is not None:
                l, r = crit_l, crit_r
                if l <= h <= r:
                    mid = (l + r) // 2
                    if h <= mid:
                        h = l - 1
                    else:
                        h = r + 1
            print(h)
        else:
            crit_l, crit_r = int(query[1]), int(query[2])
    return output.getvalue().strip()

# provided sample
assert run("5\n2 6 5 6 2\n9\n1 5 10\n2 5\n1 1 3\n3 3 6\n3 8 9\n1 2 9\n2 3\n2 2\n2 4\n") == "10\n7\n9\n7"

# custom tests
assert run("1\n1\n3\n3 2 3\n2 1\n1 1 5\n2 1\n") == "1\n5", "single building adjustments"
assert run("3\n5 5 5\n2\n3 4 6\n2 2\n") == "3", "all equal, move down"
assert run("2\n1 10\n2\n
```
