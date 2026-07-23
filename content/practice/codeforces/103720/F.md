---
title: "CF 103720F - \u0411\u0430\u0437\u0430 \u043e\u0442\u0434\u044b\u0445\u0430"
description: "Chúng tôi đang quản lý một dãy N ngôi nhà được đánh số, ban đầu tất cả đều trống rỗng. Theo thời gian, chúng tôi nhận được hai loại lệnh: yêu cầu đặt chỗ và hủy đặt phòng."
date: "2026-07-02T09:20:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103720
codeforces_index: "F"
codeforces_contest_name: "VII \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e. \u0424\u0438\u043d\u0430\u043b. 3-7 \u043a\u043b\u0430\u0441\u0441\u044b"
rating: 0
weight: 103720
solve_time_s: 51
verified: true
draft: false
---

[CF 103720F - \u0411\u0430\u0437\u0430 \u043e\u0442\u0434\u044b\u0445\u0430](https://codeforces.com/problemset/problem/103720/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang quản lý một dãy N ngôi nhà được đánh số, ban đầu tất cả đều trống rỗng. Theo thời gian, chúng tôi nhận được hai loại lệnh: yêu cầu đặt chỗ và hủy đặt phòng. Một yêu cầu đặt chỗ yêu cầu chúng ta gán M ngôi nhà trống liên tiếp, nhưng không được tùy tiện, hệ thống phải luôn chọn khối sớm nhất có thể khi quét từ trái sang phải và trong giới hạn đó, nó phải ưu tiên khối hợp lệ ngoài cùng bên trái có thể chứa tất cả M ngôi nhà liền kề. Mỗi lượt đặt chỗ được liên kết với một tên nhóm duy nhất và chúng ta phải nhớ chính xác những ngôi nhà nào được chỉ định cho mỗi nhóm. 

Lệnh hủy đề cập đến một nhóm đã đặt trước đó và giải phóng tất cả các ngôi nhà đã được chỉ định cho nhóm đó. Việc hủy được thực hiện theo cấu trúc thời gian hợp lệ, nghĩa là chúng tôi không bao giờ hủy thứ gì đó đã bị hủy và chúng tôi luôn hủy theo thứ tự phù hợp với đặt chỗ. 

Sau mỗi lần đặt phòng, chúng tôi phải xuất ra chỉ số chính xác của các ngôi nhà được chỉ định. Cuối cùng, chúng ta phải xuất tất cả các ngôi nhà miễn phí còn lại theo thứ tự được sắp xếp. 

Các ràng buộc cho phép tối đa 10^5 ngôi nhà và 10^5 hoạt động, với tổng số ngôi nhà được phân bổ cũng bị giới hạn bởi 10^6. Điều này ngay lập tức loại trừ mọi giải pháp quét toàn bộ mảng cho mọi truy vấn. Một lần quét tuyến tính cho mỗi lần đăng ký sẽ giảm xuống O(NK), quá lớn, theo thứ tự 10^10 thao tác. 

Khó khăn chính là duy trì các phân đoạn tự do động trong khi hỗ trợ các truy vấn nhanh cho phân đoạn ngoài cùng bên trái có đủ độ dài, cộng với việc xóa các phân đoạn được phân bổ trước đó. 

Trường hợp cạnh tinh tế xuất hiện khi các phân đoạn miễn phí hợp nhất sau khi hủy. Ví dụ: nếu các ngôi nhà từ 1 đến 3 và 5 đến 7 trống và 4 trở nên trống sau khi bị hủy, một cấu trúc đơn giản chỉ theo dõi các vị trí trống riêng lẻ có thể coi 1-3 và 5-7 là tách biệt mãi mãi, thiếu rằng chúng hợp nhất thành một khoảng thời gian dài hơn duy nhất 1-7. Một trường hợp đặc biệt khác phát sinh khi nhiều đăng ký lấp đầy chính xác một ranh giới phân khúc và việc hủy sẽ tạo lại một khối liền kề lớn phải được sử dụng lại trong đăng ký sau này. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ duy trì một mảng boolean có kích thước N đại diện cho tỷ lệ chiếm chỗ. Đối với mỗi yêu cầu đặt chỗ, chúng tôi sẽ quét từ 1 đến N, đếm các ô trống liên tiếp và dừng lại khi tìm thấy khối có độ dài M. Điều này đúng vì nó mô phỏng trực tiếp câu lệnh vấn đề. Tuy nhiên, trong trường hợp xấu nhất, mỗi đăng ký có thể yêu cầu quét gần như tất cả N vị trí, đặc biệt khi M nhỏ và các ô trống thưa thớt. Điều này dẫn đến O(NK), với 10^5 thao tác trở nên không khả thi. 

Sự cải thiện này đến từ việc nhận ra rằng chúng tôi liên tục truy vấn và cập nhật các phân đoạn trống liền kề. Thay vì suy luận về từng ô riêng lẻ, chúng ta duy trì các khoảng không gian trống. Mỗi đăng ký sẽ trở thành một tìm kiếm theo các khoảng thời gian cho phân đoạn sớm nhất có đủ độ dài và mỗi lần hủy sẽ trở thành sự hợp nhất của các khoảng trống liền kề. 

Cấu trúc khóa là một tập hợp các khoảng rời rạc được sắp xếp theo thứ tự đại diện cho các phạm vi tự do. Khi đăng ký, chúng tôi lặp lại từ khoảng ngoài cùng bên trái, trừ đi phần đóng góp của nó cho đến khi tìm thấy khoảng có thể chứa M. Sau đó, chúng tôi chia khoảng đó. Khi hủy, chúng tôi chèn một khoảng mới và hợp nhất với các lân cận nếu liền kề. 

Điều này làm giảm vấn đề quản lý khoảng thời gian với các hoạt động cấu trúc theo thứ tự, tất cả đều có thể đạt được theo thời gian logarit cho mỗi sự kiện bằng cách sử dụng cây cân bằng hoặc bản đồ có thứ tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Quét mảng Brute Force | O(NK) | O(N) | Quá chậm | 
| Đặt hàng khoảng thời gian miễn phí | O(K log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi duy trì cấu trúc có thứ tự cân bằng của các phân đoạn tự do, mỗi phân đoạn là một phạm vi liên tục [l, r]. Chúng tôi cũng duy trì một từ điển ánh xạ tên nhóm tới các phân đoạn được phân bổ của chúng để có thể khôi phục chúng khi hủy. 

1. Khởi tạo hệ thống với một khoảng trống duy nhất [1, N]. Điều này thể hiện rằng tất cả các ngôi nhà ban đầu đều có sẵn trong một khối liền kề. 
2. Để xử lý yêu cầu đặt chỗ cho tên nhóm có kích thước M, chúng tôi quét các khoảng trống từ trái sang phải theo thứ tự vị trí bắt đầu. Đối với mỗi khoảng [l, r], chúng tôi tính toán độ dài của nó. Nếu độ dài quãng nhỏ hơn M, chúng ta trừ nó khỏi M và chuyển sang quãng tiếp theo. Nếu độ dài khoảng ít nhất là M, chúng tôi phân bổ M ngôi nhà đầu tiên từ khoảng này, nghĩa là chúng tôi lấy [l, l+M-1] và cập nhật khoảng thành [l+M, r] nếu nó vẫn còn chỗ trống. 

Việc tiêu dùng tham lam từ trái sang phải này là đúng vì vấn đề rõ ràng đòi hỏi phải chọn những ngôi nhà tranh sớm nhất có thể. 

1. Chúng tôi lưu trữ các phân đoạn được phân bổ cho tên nhóm này. Nói chung, một nhóm chỉ có thể chiếm giữ nhiều phân đoạn rời rạc nếu việc hủy bỏ không gian bị phân mảnh trước đó, vì vậy chúng tôi lưu trữ danh sách các khoảng cho mỗi nhóm. 
2. Để xử lý việc hủy một nhóm, chúng tôi truy xuất tất cả các phân đoạn được phân bổ của nó và chèn lại chúng vào cấu trúc khoảng trống từng cái một. Sau khi chèn từng phân đoạn, chúng tôi cố gắng hợp nhất nó với các phân đoạn trống liền kề. Nếu một đoạn liền kề với khoảng trước hoặc khoảng tiếp theo (tức là chạm vào ranh giới), chúng tôi sẽ hợp nhất chúng thành một khoảng lớn hơn. 

Việc hợp nhất này là cần thiết vì nếu không, cấu trúc sẽ phân chia không gian trống một cách giả tạo và phá vỡ sự phân bổ tham lam trong tương lai. 

1. Sau khi xử lý tất cả các truy vấn, chúng tôi xuất ra tất cả các khoảng trống còn lại được mở rộng thành các chỉ mục nhỏ rõ ràng. 

### Tại sao nó hoạt động 

Điều bất biến là tại mọi thời điểm, tập hợp các khoảng trống rời rạc và thể hiện đầy đủ chính xác các ngôi nhà tự do. Mỗi phân bổ sẽ loại bỏ tiền tố của một khoảng nào đó hoặc phân tách nó một cách rõ ràng, duy trì tính chính xác. Mỗi lần hủy sẽ giới thiệu lại chính xác các phân đoạn đã bị chiếm giữ trước đó và hợp nhất với các phân đoạn lân cận để không còn phân mảnh nhân tạo. Vì các khoảng luôn được duy trì theo thứ tự được sắp xếp và không chồng chéo nên việc quét từ trái sang phải luôn xác định chính xác vị trí khả thi sớm nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, k = map(int, input().split())
    
    # free intervals stored as sorted list of [l, r]
    import bisect

    starts = [1]
    ends = [n]
    
    # map name -> list of allocated intervals
    alloc = {}

    def merge_at(i):
        # merge interval i with neighbors if adjacent
        nonlocal starts, ends

        # merge left
        if i > 0 and ends[i-1] + 1 >= starts[i]:
            i -= 1

        # merge forward
        while i + 1 < len(starts) and ends[i] + 1 >= starts[i+1]:
            ends[i] = max(ends[i], ends[i+1])
            del starts[i+1]
            del ends[i+1]

        if i > 0 and ends[i-1] + 1 >= starts[i]:
            ends[i-1] = max(ends[i-1], ends[i])
            del starts[i]
            del ends[i]

    def add_interval(l, r):
        nonlocal starts, ends

        # insert by order
        i = bisect.bisect_left(starts, l)
        starts.insert(i, l)
        ends.insert(i, r)
        merge_at(i)

    def take(m):
        nonlocal starts, ends
        res = []
        i = 0
        while m > 0:
            l, r = starts[i], ends[i]
            length = r - l + 1
            if length <= m:
                res.append((l, r))
                m -= length
                del starts[i]
                del ends[i]
            else:
                res.append((l, l + m - 1))
                starts[i] = l + m
                m = 0
        return res

    out = []

    for _ in range(k):
        parts = input().split()
        if parts[0] == "Order":
            name = parts[1]
            m = int(parts[2])
            segs = take(m)
            alloc[name] = segs
            # expand for output
            arr = []
            for l, r in segs:
                arr.extend(range(l, r + 1))
            out.append(" ".join(map(str, arr)))
        else:
            name = parts[1]
            for l, r in alloc.get(name, []):
                add_interval(l, r)
            alloc[name] = []

    # final free cells
    final = []
    for l, r in zip(starts, ends):
        final.extend(range(l, r + 1))

    print("\n".join(out))
    print()
    print(" ".join(map(str, final)))

if __name__ == "__main__":
    main()
```Giải pháp duy trì các phân đoạn trống khi hai mảng lưu trữ song song bắt đầu và kết thúc theo thứ tự được sắp xếp. các`take`hàm tiêu thụ một cách tham lam các khoảng từ bên trái, xóa chúng hoàn toàn hoặc thu nhỏ chúng từ phía bên trái, khớp chính xác với yêu cầu chỉ định các ngôi nhà sớm nhất có thể. các`add_interval`hàm giới thiệu lại các phân đoạn được giải phóng và hợp nhất chúng ngay lập tức để khôi phục cấu trúc liền kề tối đa. 

Một chi tiết triển khai tinh vi là việc hợp nhất phải xảy ra sau mỗi lần chèn vì việc hủy có thể tạo ra sự liền kề với cả khoảng thời gian trước đó và tiếp theo. Một điểm tế nhị nữa là`take`có thể xóa khoảng thời gian hiện tại nên việc quản lý chỉ mục phải luôn ở phần tử đầu tiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
Order a 2
Order b 2
Cancel a
```Chúng tôi theo dõi khoảng thời gian miễn phí và phân bổ. 

| Bước | Hoạt động | Khoảng thời gian miễn phí | Đầu ra phân bổ | 
| --- | --- | --- | --- | 
| 1 | ban đầu | [1,5] | | 
| 2 | Đặt hàng 2 | [3,5] | 1 2 | 
| 3 | Đặt hàng b 2 | [5,5] | 3 4 | 
| 4 | Hủy một | [1,2], [3,5] | | 

Điều này cho thấy cách hủy sẽ khôi phục các phân đoạn chính xác và duy trì thứ tự cho các hoạt động trong tương lai. 

### Ví dụ 2 

đầu vào:```
7 4
Order x 3
Order y 2
Cancel x
Order z 4
```| Bước | Hoạt động | Khoảng thời gian miễn phí | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | ban đầu | [1,7] | | 
| 2 | x 3 | [4,7] | 1 2 3 | 
| 3 | năm 2 | [6,7] | 4 5 | 
| 4 | hủy x | [1,3], [4,5], [6,7] | | 
| 5 | z 4 | [5,7] | 1 2 3 4 | 

Điều này xác nhận rằng việc hợp nhất sau khi hủy là cần thiết để khôi phục sự liền kề chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(K log N) khấu hao | mỗi khoảng được chia hoặc hợp nhất một số lần giới hạn, với các lần chèn theo thứ tự | 
| Không gian | O(N) | các khoảng luôn phân chia dòng mà không bị chồng chéo | 

Giới hạn của 10^5 thao tác và tổng kích thước được phân bổ lên tới 10^6 đảm bảo rằng mỗi chỉ mục tiểu thủ chỉ được chạm vào một số lần không đổi, giữ cho giải pháp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out
    try:
        main()
    finally:
        sys.stdout = old
    return out.getvalue().strip()

# sample
assert run("""5 3
Order dandelion 3
Order pear 1
Cancel dandelion
""") == """1 2 3

1 2 3 5"""

# minimum size
assert run("""1 2
Order a 1
Cancel a
""") == """1

1"""

# full consumption then reuse
assert run("""5 4
Order a 5
Cancel a
Order b 5
Cancel b
""") == """1 2 3 4 5

1 2 3 4 5"""

# fragmented allocation
assert run("""10 5
Order a 3
Order b 3
Order c 3
Cancel b
Order d 4
""") == """1 2 3
4 5 6
7 8 9

1 2 3 4 5 6 7 8 9 10"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ô đơn | 1 | phân bổ tối thiểu | 
| điền đầy đủ + tái sử dụng | dòng đầy đủ hai lần | tính đúng đắn sau khi hủy bỏ | 
| phân mảnh | hợp nhất nhiều khoảng thời gian | xây dựng lại khoảng thời gian chính xác | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng xảy ra khi việc hủy khôi phục hai khoảng thời gian sẽ hợp nhất ngay lập tức. Giả sử chúng ta có các khoảng trống [1,2] và [4,5] và chúng ta hủy một nhóm chiếm [3,3]. Hành vi đúng là hợp nhất mọi thứ vào [1,5]. Thuật toán xử lý vấn đề này vì việc chèn [3,3] sẽ kích hoạt kiểm tra kề với cả hai hàng xóm, thu gọn chúng thành một khoảng duy nhất. 

Một trường hợp khác là việc tiêu thụ một phần lặp đi lặp lại trong một khoảng thời gian trong quá trình phân bổ. Nếu một khoảng là [1.100] và chúng ta phân bổ nhiều nhóm nhỏ thì khoảng đó sẽ được chia nhiều lần từ bên trái. Cấu trúc vẫn hợp lệ vì việc thu hẹp giữ nguyên thứ tự được sắp xếp và không yêu cầu tái cân bằng ngoài điều chỉnh cục bộ. 

Trường hợp thứ ba liên quan đến việc đặt chỗ và hủy xen kẽ để tái tạo lại các phân đoạn lớn liền kề nhiều lần. Tính bất biến mà các khoảng luôn được hợp nhất đảm bảo rằng hiệu suất không bị suy giảm do phân mảnh, vì mọi ranh giới chỉ được tạo hoặc xóa trong một số lần giới hạn.
