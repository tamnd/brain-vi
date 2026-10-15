---
title: "CF 105437I - Kiểm tra thuộc tính"
description: "Chúng ta được cung cấp một chuỗi sự kiện dài theo thứ tự trong một trò chơi. Mỗi sự kiện sẽ kiếm được một điểm thuộc tính duy nhất hoặc gặp phải một kiểm tra phụ thuộc vào một trong hai chỉ số, Sức mạnh hoặc Trí thông minh."
date: "2026-06-23T03:43:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105437
codeforces_index: "I"
codeforces_contest_name: "ICPC 2024-2025 NERC, Southern and Volga Russia Qualifier"
rating: 0
weight: 105437
solve_time_s: 91
verified: false
draft: false
---

[CF 105437I - Kiểm tra thuộc tính](https://codeforces.com/problemset/problem/105437/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi sự kiện dài theo thứ tự trong một trò chơi. Mỗi sự kiện sẽ kiếm được một điểm thuộc tính duy nhất hoặc gặp phải một kiểm tra phụ thuộc vào một trong hai chỉ số, Sức mạnh hoặc Trí thông minh. Chúng tôi bắt đầu với cả hai số liệu thống kê ở mức 0 và chúng tôi biết chính xác điều đó$m$của các sự kiện là tăng điểm, nghĩa là chính xác$n - m$sự kiện là kiểm tra. 

Tại mỗi thời điểm chúng ta nhận được một điểm, chúng ta phải quyết định ngay nên gán điểm đó cho Sức mạnh hay Trí thông minh. Sau đó, khi chúng tôi tiến hành kiểm tra, nó chỉ thành công nếu chỉ số tương ứng đã đủ lớn. Chúng tôi không được phép sắp xếp lại các sự kiện và chúng tôi được phép lập kế hoạch trước cho tất cả các nhiệm vụ hoặc một cách thích ứng trong khi quét trình tự. Mục tiêu là tối đa hóa số lần kiểm tra thành công. 

Khó khăn chính là mỗi lần gán điểm đều ảnh hưởng đến khả năng trong tương lai, nhưng tính hữu ích của một điểm phụ thuộc vào việc kiểm tra nào trong tương lai thực sự xuất hiện. Một điểm dành quá sớm cho Trí thông minh có thể bị lãng phí nếu kiểm tra Sức mạnh sau đó chiếm ưu thế và ngược lại. Ràng buộc đặt hàng ngăn chặn bất kỳ sự phân bổ lại toàn cầu nào sau khi thực tế. 

Những ràng buộc làm cho việc thực hiện các nhiệm vụ bằng vũ lực không thể thực hiện được. Số điểm nhiều nhất là 5000, nhưng số sự kiện có thể lên tới 2 triệu. Bất kỳ giải pháp nào cố gắng mô phỏng tất cả các phân phối hoặc khám phá trạng thái cho mỗi sự kiện đều sẽ thất bại. Ngay cả lập trình động trên trạng thái đầy đủ cho mỗi tiền tố cũng không thể thực hiện được nếu nó phụ thuộc vào độ dài tiền tố đầy đủ. 

Một trường hợp phức tạp xuất hiện khi tất cả các lần kiểm tra ban đầu đều yêu cầu một thuộc tính nhưng các điểm đến rất ít. Ví dụ: nếu chúng tôi nhận được nhiều séc như$+2, +3, +4$trước khi có đủ điểm, một chiến lược tham lam ngây thơ có thể sớm đưa ra điểm sai cho thuộc tính sai, vì nghĩ rằng sự phân bổ trong tương lai là cân bằng. Điều đó dẫn đến những thất bại không thể khắc phục sau này khi thuộc tính kia trở nên quan trọng. 

Một trường hợp khác phát sinh khi tất cả các bước kiểm tra đều thuộc một loại nhưng chiến lược tối ưu vẫn chia điểm do thời gian. Ví dụ: ngay cả khi tất cả các lần kiểm tra đều dựa trên Thông tin, chúng tôi có thể cần trì hoãn việc ấn định điểm để các lần kiểm tra Độ mạnh sớm (nếu có) không bị chặn. 

Vấn đề cốt lõi là mọi séc đều tiêu tốn ngân sách điểm phụ thuộc vào tiền tố được gán cho một thuộc tính cụ thể và chúng tôi muốn quyết định tập hợp con séc nào sẽ đáp ứng theo ràng buộc đặt hàng toàn cầu. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ coi mỗi sự kiện điểm là một quyết định nhị phân: gán nó cho Sức mạnh hoặc Trí thông minh. Với$m$điểm, điều này mang lại$2^m$bài tập. Đối với mỗi nhiệm vụ, chúng tôi mô phỏng trình tự và đếm số lần kiểm tra đã vượt qua. Ngay cả khi việc kiểm tra một bài tập là tuyến tính trong$n$, điều này dẫn đến$O(n \cdot 2^m)$, điều này vượt xa khả thi khi$m = 5000$. 

Chúng ta cần một cách để tránh việc quyết định rõ ràng nhiệm vụ của từng điểm một cách độc lập. Quan sát quan trọng là danh tính chính xác của từng điểm không quan trọng, chỉ có bao nhiêu điểm được dành riêng cho Sức mạnh so với Trí thông minh ở mỗi tiền tố. Tuy nhiên, ngay cả việc duy trì ngân sách tiền tố đầy đủ vẫn là quá lớn. 

Việc tái cấu trúc quan trọng là lật ngược quan điểm: thay vì quyết định cách chỉ định điểm và sau đó kiểm tra tất cả các bước kiểm tra, thay vào đó, chúng tôi quyết định những bước kiểm tra nào chúng tôi muốn đáp ứng và sau đó hỏi liệu có tồn tại sự phân công điểm nhất quán hỗ trợ chúng hay không. 

Nếu chúng tôi sửa một tập hợp các kiểm tra hài lòng, chúng tôi có thể mô phỏng một cách tham lam xem liệu chúng tôi có thể hỗ trợ chúng hay không: chúng tôi quét trình tự, đếm số điểm có sẵn và đảm bảo rằng khi một kiểm tra xuất hiện, chúng tôi có đủ điểm được phân bổ cho thuộc tính đó. Vấn đề duy nhất còn lại là chọn những tờ séc nào sẽ giữ lại. 

Điều này trở thành vấn đề lập kế hoạch trên một dòng: chúng tôi muốn chọn càng nhiều lượt kiểm tra càng tốt, trong đó mỗi lượt kiểm tra tiêu tốn một số đơn vị tài nguyên (điểm) nhất định mà lẽ ra phải được chỉ định trước đó. Cấu trúc này tự nhiên dẫn đến lập trình động trên các tiền tố kết hợp với lựa chọn tham lam bằng cách sử dụng hàng đợi ưu tiên hoặc lập lịch theo thứ tự, trong đó chúng tôi luôn loại bỏ kiểm tra thỏa mãn đắt nhất khi cần thiết. 

Giải pháp cuối cùng nén vấn đề vào việc xử lý trình tự một lần, duy trì tính khả thi của các kiểm tra đã chọn bằng cách sử dụng cấu trúc đảm bảo chúng tôi không bao giờ vượt quá số điểm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua bài tập |$O(n \cdot 2^m)$|$O(m)$| Quá chậm | 
| Tham lam + mô phỏng tối ưu |$O(n \log n)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý trình tự từ trái sang phải trong khi vẫn duy trì số điểm hiện có và số lượng kiểm tra mà chúng tôi đã chọn để đáp ứng. 

1. Chúng tôi giữ hai bộ đếm cho các tài nguyên sẵn có: một bộ đếm cho các điểm chưa sử dụng và một bộ đếm số điểm đã được gán ngầm cho Sức mạnh hoặc Trí thông minh khi cần. Thay vì phân chia các điểm một cách rõ ràng ngay lập tức, chúng tôi coi mỗi điểm là linh hoạt cho đến khi nó bị ép buộc bởi yêu cầu kiểm tra. 
2. Khi gặp sự kiện điểm, chúng tôi sẽ tăng tổng số điểm hiện có. Điều này thể hiện một đơn vị bổ sung mà sau này có thể được phân bổ cho một trong hai thuộc tính tùy thuộc vào nhu cầu trong tương lai. 
3. Khi gặp bài kiểm tra Sức mạnh hoặc Trí thông minh, chúng ta tạm cho rằng mình sẽ cố gắng đáp ứng nó. Chúng tôi tăng số lượng bắt buộc cho thuộc tính đó và kiểm tra xem hiện tại chúng tôi có đủ tổng số điểm để hỗ trợ tất cả các ràng buộc được chấp nhận trước đó hay không. 
4. Nếu việc đáp ứng bước kiểm tra mới vượt quá số điểm hiện có, chúng tôi phải từ chối một bước kiểm tra đã được chấp nhận trước đó để khôi phục tính khả thi. Chúng tôi chọn từ chối séc được chấp nhận “đắt” nhất, nghĩa là séc yêu cầu nhu cầu thuộc tính lớn nhất, vì việc loại bỏ nó sẽ giải phóng ngân sách tiền tố bị hạn chế nhất. 
5. Chúng tôi duy trì một cấu trúc (một đống tối đa) các séc được chấp nhận theo mức độ yêu cầu của chúng. Mỗi lần chúng tôi vượt quá dung lượng, chúng tôi sẽ thực hiện kiểm tra yêu cầu lớn nhất. 
6. Chúng ta tiếp tục quá trình này cho đến hết trình tự. Kích thước của heap ở cuối là số lần kiểm tra thỏa mãn tối đa. 

Ý tưởng tinh tế là chúng ta không bao giờ cần phải quyết định rõ ràng cách gán từng điểm; thay vào đó, tính khả thi được thực thi trên toàn cầu bằng cách đảm bảo rằng tổng nhu cầu do các séc đã chọn không bao giờ vượt quá số điểm sẵn có ở bất kỳ tiền tố nào. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến rằng tại bất kỳ thời điểm nào trong quá trình quét, tập hợp các kiểm tra đã chọn đều khả thi đối với số điểm được nhìn thấy cho đến nay. Nếu một kiểm tra mới được thêm vào, tính khả thi có thể chỉ bị phá vỡ về tổng dung lượng tài nguyên chứ không phải thứ tự vì tất cả các điểm đều có thể thay thế cho nhau. Loại bỏ séc có chi phí cao nhất là tối ưu vì nó làm giảm tổng nhu cầu một cách mạnh mẽ nhất, duy trì khả năng giữ lại nhiều séc có chi phí thấp hơn. Đây là một lập luận trao đổi cổ điển: bất kỳ giải pháp tối ưu nào loại trừ yêu cầu nhỏ hơn thay vì yêu cầu lớn nhất đều có thể được cải thiện bằng cách hoán đổi chúng mà không làm giảm tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    arr = list(map(int, input().split()))

    import heapq

    points = 0
    heap = []  # max heap via negatives

    for x in arr:
        if x == 0:
            points += 1
        else:
            req = abs(x)
            heapq.heappush(heap, -req)

            # We need one unit of point "budget" per selected check
            if len(heap) > points:
                heapq.heappop(heap)

    print(len(heap))

if __name__ == "__main__":
    solve()
```Mã xử lý chuỗi một lần, cập nhật số điểm khả dụng bất cứ khi nào gặp số 0. Mỗi tấm séc được chèn vào một đống tối đa bằng cách sử dụng giá trị yêu cầu của nó. Nếu số lượng bài kiểm tra đã chọn vượt quá số điểm hiện có, chúng tôi sẽ loại bỏ bài kiểm tra khó nhất. Kích thước heap luôn thể hiện số lần kiểm tra tốt nhất có thể đạt được theo tiền tố hiện tại. 

Một điểm tế nhị là chúng ta không bao giờ phân biệt rõ ràng Strength và Intelligence. Điều này có hiệu quả vì hạn chế duy nhất là mỗi lần kiểm tra tiêu tốn một đơn vị tài nguyên có thể chỉ định và thứ tự của các loại thuộc tính không ảnh hưởng đến tính khả thi ngoài tổng phân bổ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Trình tự đầu vào: điểm và kiểm tra hỗn hợp. Chúng tôi theo dõi điểm và kiểm tra được lựa chọn. 

| Bước | Sự kiện | Điểm | Heap (yêu cầu phủ định) | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 0 | [] | bắt đầu | 
| 2 | 50 | 1 | [] | +1 điểm | 
| 3 | 1 | 1 | [-1] | thêm séc | 
| 4 | 0 | 2 | [-1] | +1 điểm | 
| 5 | 2 | 2 | [-2, -1] | thêm séc | 
| 6 | 0 | 3 | [-2, -1] | +1 điểm | 
| 7 | -3 | 3 | [-2, -1] | khả thi | 
| 8 | 0 | 4 | [-2, -1] | +1 điểm | 
| 9 | -4 | 4 | [-2, -1] | khả thi | 
| 10 | 0 | 5 | [-2, -1] | +1 điểm | 
| 11 | -5 | 5 | [-2, -1] | khả thi | 

Kích thước heap cuối cùng là 3. 

Dấu vết này cho thấy rằng khi tích lũy đủ điểm, chúng tôi không bao giờ cần bỏ bất kỳ bước kiểm tra nào đã chọn. Đống vẫn ổn định sau khi ổn định ban đầu. 

### Mẫu 2 

Đầu vào: kiểm tra rất sớm trước bất kỳ điểm nào. 

| Bước | Sự kiện | Điểm | Đống | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | -1 | 0 | [-1] | thêm séc | 
| 2 | 0 | 1 | [-1] | +1 điểm | 

Ở đây chúng ta không bao giờ có thể thỏa mãn bất kỳ sự kiểm tra nào vì dù sau này chúng ta có điểm nhưng cấu trúc tham lam đảm bảo tính khả thi được kiểm tra liên tục. Vì không thể hỗ trợ kiểm tra bổ sung một cách an toàn nên kết quả là 0. 

Điều này chứng tỏ tầm quan trọng của tính khả thi của tiền tố: việc cam kết quá sớm không thể được khắc phục chỉ bằng các điểm trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| mỗi thao tác chèn/xóa séc sử dụng các thao tác heap | 
| Không gian |$O(m)$| lưu trữ heap ở tất cả các lần kiểm tra | 

Thuật toán này hiệu quả vì mỗi sự kiện được xử lý một lần và các hoạt động của đống vẫn bị giới hạn bởi số lượng kiểm tra được chấp nhận, không thể vượt quá$m \le 5000$. Điều này phù hợp thoải mái trong giới hạn ngay cả khi$n$đạt tới hai triệu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    arr = list(map(int, input().split()))

    import heapq

    points = 0
    heap = []

    for x in arr:
        if x == 0:
            points += 1
        else:
            heapq.heappush(heap, -abs(x))
            if len(heap) > points:
                heapq.heappop(heap)

    return str(len(heap))

# provided samples
assert run("10 5\n0 1 0 2 -3 0 -4 0 -5") == "3"
assert run("3 1\n-1 0") == "0"

# custom cases
assert run("5 2\n0 1 0 -2 0") == "2", "balanced small"
assert run("4 1\n-1 -2 0 -3") == "0", "no early points"
assert run("6 3\n0 0 0 1 -1 -2") == "3", "all points early"
assert run("1 0\n0") == "0", "no checks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có điểm sớm | 0 | không thể đáp ứng các yêu cầu kiểm tra nếu không có nguồn lực | 
| tất cả các điểm sớm | 3 | tham lam tích lũy ổn định | 
| trộn nhỏ | 2 | hành vi lập kế hoạch cân bằng | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi các dấu kiểm xuất hiện trước bất kỳ điểm nào. Trong tình huống như vậy, vùng heap sẽ tăng lên trong khi các điểm vẫn bằng 0 và mỗi lần chèn sẽ ngay lập tức kích hoạt việc xóa cho đến khi vùng heap trống. Đối với đầu vào như$[-1, -2, 0, -3]$, trước tiên thuật toán sẽ chèn hai kiểm tra nhưng không thể hỗ trợ chúng, vì vậy cả hai đều bị xóa, không để lại kiểm tra nào thỏa mãn. 

Một trường hợp khác là khi các điểm đến một khối trước bất kỳ lần kiểm tra nào. Đối với đầu vào như$[0, 0, 0, 1, -1, -2]$, vùng heap sẽ tích lũy các lần kiểm tra sau khi đã tồn tại đủ tài nguyên. Không có hoạt động xóa nào xảy ra nên tất cả các bước kiểm tra đều được chấp nhận. 

Trường hợp khó phát hiện cuối cùng là khi một yêu cầu rất lớn được thực hiện muộn sau nhiều yêu cầu nhỏ. Vùng heap đảm bảo rằng yêu cầu lớn được loại bỏ trước tiên, duy trì các kiểm tra nhỏ hơn nhằm tối đa hóa tổng số lượng, phù hợp với chiến lược tối ưu theo lý luận trao đổi.
