# Class Definitions

class Job:
    def _init_(self, job_id, title, description, client, budget):
        self.job_id = job_id
        self.title = title
        self.description = description
        self.client = client
        self.budget = budget
        self.bids = []
        self.is_open = True

    def post_bid(self, bid):
        if self.is_open:
            self.bids.append(bid)
            print(f"Bid by {bid.freelancer.name} has been posted.")
        else:
            print("Job is closed for bidding.")

    def close_job(self):
        self.is_open = False
        print(f"Job '{self.title}' has been closed for bidding.")


class Freelancer:
    def _init_(self, freelancer_id, name):
        self.freelancer_id = freelancer_id
        self.name = name
        self.bids = []

    def place_bid(self, job, amount):
        if job.is_open:
            bid = Bid(len(job.bids) + 1, self, job, amount)
            job.post_bid(bid)
            self.bids.append(bid)
        else:
            print("Cannot place bid, job is closed.")


class Client:
    def _init_(self, client_id, name):
        self.client_id = client_id
        self.name = name
        self.jobs = []

    def post_job(self, title, description, budget):
        job = Job(len(self.jobs) + 1, title, description, self, budget)
        self.jobs.append(job)
        print(f"Job '{title}' has been posted.")

    def choose_bid(self, job, bid):
        if bid in job.bids:
            print(f"Bid by {bid.freelancer.name} has been accepted.")
            job.close_job()
        else:
            print("Bid not found in job bids.")


class Bid:
    def _init_(self, bid_id, freelancer, job, amount):
        self.bid_id = bid_id
        self.freelancer = freelancer
        self.job = job
        self.amount = amount

client1 = Client(1, "Client1")
freelancer1 = Freelancer(1, "Freelancer1")
freelancer2 = Freelancer(2, "Freelancer2")

client1.post_job("Build a Website", "Create a responsive website", 1000)

freelancer1.place_bid(client1.jobs[0], 900)
freelancer2.place_bid(client1.jobs[0], 850)

client1.choose_bid(client1.jobs[0], client1.jobs[0].bids[1])
