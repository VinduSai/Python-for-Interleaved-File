#!/usr/bin/env python3
import argparse
from Bio import SeqIO

def interleave_fastq(forward_file, reverse_file, output_file):

    # For createing iterators for the forward and reverse FASTQ files
    forward_reads = SeqIO.parse(forward_file, "fastq")
    reverse_reads = SeqIO.parse(reverse_file, "fastq")

    # for opening the output file for writing and interleave the records
    with open(output_file, 'w') as output_handle:
        # Use a generator to yield interleaved pairs of forward and reverse reads
        interleaved_pairs = ((forward, reverse) for forward, reverse in zip(forward_reads, reverse_reads))

        # for writing the interleaved records to the output file
        for forward_record, reverse_record in interleaved_pairs:
            SeqIO.write(forward_record, output_handle, "fastq")
            SeqIO.write(reverse_record, output_handle, "fastq")

def main():
    # Set up argument parser
    parser = argparse.ArgumentParser(description='Interleave Paired-End FASTQ Files')
    parser.add_argument('forward_file', help='Forward reads FASTQ file (e.g., bacterium_R1.fastq)')
    parser.add_argument('reverse_file', help='Reverse reads FASTQ file (e.g., bacterium_R2.fastq)')
    parser.add_argument('output_file', help='Output interleaved FASTQ file')

    # Parse the command-line arguments and call the interleave function
    args = parser.parse_args()
    interleave_fastq(args.forward_file, args.reverse_file, args.output_file)

if __name__ == '__main__':
    main()
